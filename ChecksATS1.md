# Check pre-production

## Context

we're resolving the issues tracked in ALIFR-243/250/188/109
The solution found is to change the uniqueId and the mic so that quanthouse rules are conserved.

mic <- segmentMic | operatingMic | FOSMarketID

In many segments there is no issue on the mic (FOSMarketID = operatingMic, and sometimes even operatingMic = segmentMic). But on some stocks of NOMX (FNSE,XSTO,...) and EURONEXT (XPAR,ALXP,...) we have a situation of the following.

```xml
	<stock name="EVO" description="Evolution Gaming Group AB" instrumentCode="929040170" isin="SE0006826046" FOSMarketId="FNSE" SegmentMic="" OperatingMic="XSTO" mic="FNSE" bsym="EVO" currency="SEK" symbol="EVO" lotSize="1" priceUnit="0" tickRule="MiFID II tick size table (600)[4391045]" internalSourceId="67" mifid2="true" liquidityInd="true" operatingMic="XSTO" fosMarketID="443" segmentMic="" uniqueId="SE0006826046@XSTO"/>
```

MIC was defined as FOSMarketID, and UniqueID has a UniqueIDPolicy that takes it to be isin+'@'+mic. If MIC != SegmentMic and segmentMic!= operatingMic, we have a double problem:

- subscription fails (summary disposed)
- Orders fail to send (wrong tag 207)

We develop a quick check based on GetDictionaryData.py to verify that our workaround will not impact non-problematic stocks.
 
## The fix

The fix consists in the following script:

```javascript
function addAttributes(el) {
        if (el.isInstrument()){
            var isin = el.getAttribute('isin');
            var mic = el.getAttribute('mic');
            var name = el.getAttribute('name');
            var operatingmic = el.getAttribute('OperatingMic');
            var segmentmic = el.getAttribute('SegmentMic');
                //Override uniqueId configuration for Quanthouse connection INGALI-92
            if ((isin != null) && (mic != 'XTKS') && (mic != 'XEUR'))
            {
                if ( (operatingmic == 'XSTO') || (operatingmic == 'FNSE') || (operatingmic == 'XHEL') || (operatingmic == 'XSAT') || (operatingmic == 'XCSE') || (operatingmic == 'XOSL') || (operatingmic == 'XPAR') || (operatingmic == 'ALXP') || (operatingmic == 'XMLI') )
                { // If we are on NOMX or EURONEXT and there is an operating MIC: we use the available segmentMIC
                    if ( (segmentmic == '') || (segmentmic ==null) )
                    {// if the SegmentMIC is not available or not assigned: use operatingMIC
                        el.setString('uniqueId',isin+'@'+operatingmic);
                        el.setString('mic',operatingmic);
                    }
                    else {
                        el.setString('uniqueId',isin+'@'+segmentmic);
                        el.setString('mic',segmentmic);
                    }
                }// If we are not on NOMX or EURONEXT: for now, keep using MIC, until ingalys provides something OK.
                else
                    el.setString('uniqueId',isin+'@'+mic);
            }// backup case for when there is no ISIN:
            else
                el.setString('uniqueId',name+'@'+mic);
        }
}

function declareAttributes(meta) {

}
```

## The checks

We want that stocks with non-problematic IDs (i.e. the ones on which we can already send orders) are not affected.

For all stocks in stock visu, we print the current couple (mic,fosmarketid,operatingmic,segmentmic).

These stocks are available in Common/Nicolas/Dictionnaire/checksATS1 as stocks.xlsx

These checks highlighted the need for a better output. -> class? modulewriteoutput? other? to be developed later.
We also want to apply some sort of filter per exchange: 1. to avoid reading all the dictionaries, and 2. to avoid duplicates across dictionaries (e.g. the same stock trading on three different markets)
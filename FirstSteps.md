# First steps with the QuantHouse Toolbox

### Looking for instrument data on the front end

One can use the FeedOs documentation provided by QuantHouse.

#### Get instrument details.

'''shell
# equivalent function on the FeedOS API: REFERENTIAL_GetInstruments 
./toolbox/feedos_cli 172.16.112.86 6051 alize (pwd) geti 499123198 # In the FeedOs Toolbox
'''

###### Output:

'''
instr # 238/1022 = 499123198
    PriceCurrency            	string{EUR}
    Symbol                   	string{SAN}
    Description              	string{BANCO SANTANDER S.A.}
    SecurityType             	string{CS}
    FOSMarketId              	XMCE
    ContractMultiplier       	float64{1}
    CFICode                  	string{ESXXXX}
    RoundLot                 	float64{1}
    MinTradeVol              	float64{1}
    SecurityGroup            	string{AC}
    MarketSegmentID          	string{1}
    InternalCreationDate     	Timestamp{2013-04-10 05:00:13:246}
    InternalModificationDate 	Timestamp{2018-04-15 14:19:03:952}
    InternalSourceId         	uint16{89}
    InternalAggregationId    	uint16{3}
    InternalEntitlementId    	int32{1012}
    LastTradingDate          	Timestamp{2017-11-15}
    FIGI                     	string{BBG000K65GY5}
    FISN                     	string{SAN/AC 0,50}
    MiFID_Flags              	uint16{3}
    AverageDailyTurnover     	float64{767213906.542019}
    AverageValueOfTransactions	float64{18817.8625283167}
    StandardMarketSize       	uint32{10000}
    AverageDailyNumberOfTransactions	float64{16855.3350253807}
    MiFID_MostRelevantMarket 	string{XMAD}
    PostTradeLIS             	float64{650000}
    LocalCodeStr             	string{ES0113900J37}
    ISIN                     	string{ES0113900J37}
    PriceDisplayPrecision    	int16{3}
    BloombergTicker          	string{SAN}
    PriceIncrement_dynamic_TableId	uint32{5832822}
    OperatingMIC             	string{BMEX}
    SegmentMIC               	string{XMAD}
    CCP_Eligible             	bool{False}
    DynamicVariationRange    	float64{2}
    StaticVariationRange     	float64{6}
SubscribeInstrumentMBL_Started
ticket = 18795
OF 238/1022
OF 238/1022 L(0) bid(5) ask(5) TimesUTC(Market=null,Server=2018-05-07 13:54:41:208) MVD=10
	 0	BID    5.3790 x   7564 @      4	ASK    5.3820 x  10569 @      4
	 1	BID    5.3780 x  22893 @      6	ASK    5.3830 x  13213 @      4
	 2	BID    5.3770 x  11045 @      5	ASK    5.3840 x  13660 @      6
	 3	BID    5.3760 x  36101 @      9	ASK    5.3850 x  34291 @      6
	 4	BID    5.3750 x  32996 @      9	ASK    5.3860 x  10093 @      4
	 5	BID **************************	ASK **************************
OD 238/1022 L(0) 	2018-05-07 13:54:42:976.694 /ServerUTCTime: 2018-05-07 13:54:42:999
BidChangeQtyAtLevel level=0 price=0 qty=8428 nb_orders=5
OD 238/1022 L(0) 	2018-05-07 13:54:45:329.001 /ServerUTCTime: 2018-05-07 13:54:45:357
BidChangeQtyAtLevel level=0 price=0 qty=11348 nb_orders=6
OR 238/1022 L(0) 	bid(0*5) ask(0+0) null                    /ServerUTCTime: 2018-05-07 13:54:45:758
	 0	BID    5.3800 x   3958 @      3	ASK                           
	 1	BID    5.3790 x  11348 @      6	ASK                           
	 2	BID    5.3780 x  21963 @      5	ASK                           
	 3	BID    5.3770 x   9733 @      5	ASK                           
	 4	BID    5.3760 x  36101 @      9	ASK                           
	 5	BID **************************	ASK                           
OD 238/1022 L(0) 	null                    /ServerUTCTime: 2018-05-07 13:54:46:253
BidChangeQtyAtLevel level=3 price=0 qty=8505 nb_orders=4
OD 238/1022 L(0) 	null                    /ServerUTCTime: 2018-05-07 13:54:46:662
BidChangeQtyAtLevel level=3 price=0 qty=14127 nb_orders=5
'''

#### Get the Level 1 InstrumentStatus

'''
SubscribeInstrumentL1 <instrument_code>
'''

As we can see below there is at least two different types of events below, after the instrument information: One of them is an update on LastTradePrice and LastTradeQty, the other an update on BestAsk/Bid. In one of the cases the MarketTimestamps are known to the second, and in the other case they are known to the millisecond. Why?
See below:

'''
SubscribeInstrumentL1_Started
ticket = 249039
InstrumentStatusL1
-- 238/1022            
	BID: 5.376	8921	@4
	ASK: 5.378	4660	@3
        LastPrice                	float64{5.377}
        LastTradeQty             	float64{810}
        DailyHighPrice           	float64{5.405}
        DailyLowPrice            	float64{5.356}
        DailyTotalVolumeTraded   	float64{87263067}
        DailyTotalAssetTraded    	float64{469987187.349799}
        LastTradePrice           	float64{5.377}
        LastTradeTimestamp       	Timestamp{2018-05-07 14:54:46:016}
        InternalDailyOpenTimestamp	Timestamp{2018-05-07 07:00:17:016}
        InternalDailyCloseTimestamp	Timestamp{2018-05-04 15:38:00:022}
        InternalDailyHighTimestamp	Timestamp{2018-05-07 09:46:37:130}
        InternalDailyLowTimestamp	Timestamp{2018-05-07 14:44:38:294}
        InternalPriceActivityTimestamp	Timestamp{2018-05-07 14:55:01:076}
        PriceActivityMarketTimestamp	Timestamp{2018-05-07 14:55:01:062}
        MidPrice                 	float64{5.377}
        TradingStatus            	17=ReadyToTrade
        LastOffBookTradePrice    	float64{5.39}
        LastOffBookTradeQty      	float64{2735337}
        LastOffBookTradeTimestamp	Timestamp{2018-05-07 12:00:12:217}
        SessionVWAPPrice         	float64{5.3845}
        DailyOpeningPrice        	float64{5.387}
        PreviousDailyTotalVolumeTraded	float64{34878071}
        PreviousDailyTotalAssetTraded	float64{186644516.6601}
        VarClose                 	float64{0.447}
        VarClosePct              	float64{9.06693711967546}
        PreviousDailyClosingPrice	float64{5.376}
        PreviousBusinessDay      	Timestamp{2018-05-04}
        CurrentBusinessDay       	Timestamp{2018-05-07}
        PreviousDailySettlementPrice	float64{4.93}
        LastAuctionPrice         	float64{5.387}
        LastAuctionVolume        	float64{218953}
        DailyTotalOffBookVolumeTraded	float64{77364718}
        DailyTotalOffBookAssetTraded	float64{416689209.5038}
        PreviousInternalDailyClosingPriceType	char{a}
        DailyHighBidPrice        	float64{5.404}
        DailyLowBidPrice         	float64{5.355}
        DailyHighAskPrice        	float64{5.405}
        DailyLowAskPrice         	float64{5.357}
        DailyHighMidPrice        	float64{5.4045}
        DailyLowMidPrice         	float64{5.356}
        DailyNumberOfBlockTrades 	uint32{99}
        DailyTotalBlockVolumeTraded	float64{1574447}
        DailyNumberOfTrades      	uint32{6054}
        PreviousDailyHighPrice   	float64{5.379}
        PreviousDailyLowPrice    	float64{5.301}
        PreviousValidBidTimestamp	Timestamp{2018-05-07 14:55:01:062}
        PreviousValidAskTimestamp	Timestamp{2018-05-07 14:54:46:185}
        DailyClosingBidPrice     	float64{5.376}
        DailyClosingAskPrice     	float64{5.377}
        InternalLastAuctionTimestamp	Timestamp{2018-05-07 13:55:43:252}
        InternalDailyBusinessDayTimestamp	Timestamp{2018-05-07 07:00:17:016}
        DailyClosingBidTimestamp 	Timestamp{2018-05-04 15:35:15:001}
        DailyClosingAskTimestamp 	Timestamp{2018-05-04 15:35:15:001}
        PreviousSettlementPriceType	char{a}
        DailyOpeningPriceTimestamp	Timestamp{2018-05-07 07:00:17:002}
        DailyHighPriceTimestamp  	Timestamp{2018-05-07 09:46:37:115}
        DailyLowPriceTimestamp   	Timestamp{2018-05-07 14:44:38:280}
        PreviousDailyClosingPriceTimestamp	Timestamp{2018-05-04 15:38:00}
        StaticTradingReferencePrice	float64{5.387}
        MARKET_BME_DynamicVariationRange	float64{2.5}
        MARKET_BME_StaticVariationRange	float64{6}
EV 238/1022            	2018-05-07 14:55:09     /ServerUTCTime: 2018-05-07 14:55:09:060.953
content: LastPrice LastTradeQty OtherValues
	LastTradeQty  = 1759
	LastPrice     = 5.376
CONTEXT: 
TradeID:	010012979
	TradeExecutionTimestamp:	Timestamp{2018-05-07 14:55:09:027}
	TradeExecutionVenue:	BMEX
	MARKET_BME_TradeTypeIndicator:	1
VALUES:
    DailyNumberOfTrades      	uint32{6055}
    VarClose                 	float64{0.446000000000001}
    VarClosePct              	float64{9.04665314401624}

EV 238/1022            	2018-05-07 14:55:09:047.738 /ServerUTCTime: 2018-05-07 14:55:09:062.087
content: Bid Ask OtherValues
	BestBid       = 5.376	7624	@4
	BestAsk       = 5.378	7160	@4
VALUES:
    PreviousValidBidTimestamp	Timestamp{2018-05-07 14:55:09:047}
    PreviousValidAskTimestamp	Timestamp{2018-05-07 14:55:09:047}
'''

# Get Dictionary Data .py

## Read Input File

My goal is to take the input file (a csv file looking like this):

```bash
E000A1X3X33
GB00BD6GN030
DE000A11QW50
FR0000066722
FR0000036816
IT0001033700
GB0005622542
DE0005489561
LU1287023185
US0970231058
[...]
US1667641005
JE00BYVQYS01
LU0488317701
SE0000683484
FR0013204351
IT0004171440
SE0009973449
FR0000120404
GB00BDFXHW57
IT0004056880
```

We want to turn that list into a python readable list of ISIN. 
We also want to strip MICs if they exist.
We know that some ISIN will have multiple matches in the dictionary, but this is not a reason to sweat for now.

I defined a module modulereadinputfile that reads the input file argument in the main script (the first argument) and feeds it to python to make a list.

In this module is a ReadInput function that will form the list:

```python

## open the input file and make it a Python List

def ReadInput(myInputFile=None):
    inputFile = PrintInput(myInputFile)
    content = [line.rstrip('\n') for line in open(inputFile)]
    content2 =[]
    for word in content:
        if "@" in word:
            word = word[:-5]
        content2.append(word)

    if any("@" in word for word in content2):
        print "ho!"

    print "the list of",len(content2),"ISINs is: ",content2
    return content2

```

Indeed the ```with``` statement in python can be dangerous in big data as it could cause a MemoryError. better use a for loop in these instances.

I've added a check for mic presence... that's not perfect, because it simply drops the MIC if was given, but this is probably enough for this exercise.

## read the dictionary files and parse their data into lists.

This one will have some xml in it. First we get to where the dict files are located.
This requires scanning the folders to skip the wrong ones and get only to the proper driver-dicts.
We'll once again rely on the powers of XML ElementTree python libs.

For now, we settle for the following tools:

```python

	# this will print the mic of any instrument matching with the if statement.
   print [child.get('mic') for child in root.findall('.//mic/*[@isin]') if child.get('uniqueId') == "GB00BFM6RT85@XLON"]

    # this will print a list of pairs (uniqueId,mic) for every instrument in file where the MIC field is different from the mic used in making the uniqueId:
   print  [(child.get('uniqueId'),child.get('mic')) for child  in root.findall('.//mic/*[@isin]') if child.get('mic') != \
child.get('uniqueId')[-4:]]

    # output for this last command on qh-quantfeed-europe
  [('FO0000000179@ONSE', 'FNSE'), ('NO0003080608@ONSE', 'FNSE'), ('NO0003035305@ONSE', 'FNSE'), ('NO0010199052@ONSE', 'FNSE'), ('CY0102630916@ONSE', 'FNSE'), ('NO0003679102@ONSE', 'FNSE'), ('NO0010360266@ONSE', 'FNSE'), ('NO0003399917@ONSE', 'FNSE'), ('BMG671801022@ONSE', 'FNSE'), ('SG1AD2000008@ONSE', 'FNSE'), ('CY0101550917@ONSE', 'FNSE'), ('KYG813131011@ONSE', 'FNSE'), ('SE0006091997@XSTO', 'FNSE'), ('SE0008347660@XSTO', 'FNSE'), ('NO0010657448@ONSE', 'FNSE'), ('NO0010694029@ONSE', 'FNSE'), ('NO0010576010@ONSE', 'FNSE'), ('NO0010283211@ONSE', 'FNSE'), ('DK0060477263@ONSE', 'FNSE'), ('NO0010564701@ONSE', 'FNSE'), ('NO0010776875@ONSE', 'FNSE'), ('SE0002367797@XSTO', 'FNSE'), ('NO0003064107@ONSE', 'FNSE'), ('NO0010716418@ONSE', 'FNSE'), ('NO0010317340@ONSE', 'FNSE'), ('NO0010014632@ONSE', 'FNSE'), ('SE0005992831@XSTO', 'FNSE'), ('SE0007730650@XSTO', 'FNSE'), ('SE0001105511@XSTO', 'FNSE'), ('NO0010571680@ONSE', 'FNSE'), ('NO0003095309@ONSE', 'FNSE'), ('NO0010792625@ONSE', 'FNSE'), ('NO0010671068@ONSE', 'FNSE'), ('SE0005003654@XSTO', 'FNSE'), ('NO0010466022@ONSE', 'FNSE'), ('NO0010289200@ONSE', 'FNSE'), ('NO0003043309@ONSE', 'FNSE'), ('NO0010629108@ONSE', 'FNSE'), ('DK0060945467@ONSE', 'FNSE'), ('NO0010593544@ONSE', 'FNSE'), ('BMG1466R1088@ONSE', 'FNSE'), ('NO0010734338@ONSE', 'FNSE'), ('NO0003070609@ONSE', 'FNSE'), ('SE0007100342@XSTO', 'FNSE'), ('NO0010778095@ONSE', 'FNSE'), ('NO0010743545@ONSE', 'FNSE'), ('NO0003096208@ONSE', 'FNSE'), ('NO0003117202@ONSE', 'FNSE'), ('NO0003025009@ONSE', 'FNSE'), ('SE0000188518@XSTO', 'FNSE'), ('NO0010360175@ONSE', 'FNSE'), ('NO0010571698@ONSE', 'FNSE'), ('SE0006543344@XSTO', 'FNSE'), ('SE0007158928@XSTO', 'FNSE'), ('NO0010001118@ONSE', 'FNSE'), ('NO0010397581@ONSE', 'FNSE'), ('NO0004913609@ONSE', 'FNSE'), ('MT0001000109@XSTO', 'FNSE'), ('SE0008091904@XSTO', 'FNSE'), ('CY0101162119@ONSE', 'FNSE'), ('NO0010789506@ONSE', 'FNSE'), ('SE0006143129@XSTO', 'FNSE'), ('SE0009160872@XSTO', 'FNSE'), ('CA46016U1084@XSTO', 'FNSE'), ('NO0010284318@ONSE', 'FNSE'), ('NO0010739402@ONSE', 'FNSE'), ('NO0010159684@ONSE', 'FNSE'), ('SE0003366871@ONSE', 'FNSE'), ('NO0004895103@ONSE', 'FNSE'), ('DK0060520450@ONSE', 'FNSE'), ('NO0010763550@ONSE', 'FNSE'), ('NO0010257728@ONSE', 'FNSE'), ('NO0010650013@ONSE', 'FNSE'), ('NO0010299068@ONSE', 'FNSE'), ('NO0003079709@ONSE', 'FNSE'), ('SE0007074505@XSTO', 'FNSE'), ('SE0007277876@XSTO', 'FNSE'), ('NO0010550056@ONSE', 'FNSE'), ('NO0010429145@ONSE', 'FNSE'), ('NO0003572802@ONSE', 'FNSE'), ('NO0010781206@ONSE', 'FNSE'), ('SE0006826046@XSTO', 'FNSE'), ('NO0010808892@ONSE', 'FNSE'), ('FO000A0DN9X4@ONSE', 'FNSE'), ('NO0010598683@ONSE', 'FNSE'), ('NO0003055808@ONSE', 'FNSE'), ('NO0010209331@ONSE', 'FNSE'), ('NO0003399909@ONSE', 'FNSE'), ('SE0004840718@XSTO', 'FNSE'), ('NO0003078107@ONSE', 'FNSE'), ('NO0010715394@ONSE', 'FNSE'), ('NO0010387004@ONSE', 'FNSE'), ('NO0010379779@ONSE', 'FNSE'), ('NO0003103103@ONSE', 'FNSE')]

```

## problem solving

we've got two types of issues: 
	
	1. a list of stocks is given and we want to recover them
	2. our list is not given and we have to book-keep the stocks in the release.

Right now we're more into situation #1. 

There's a python main script that reads a list of stocks (can take the production list of stocks as input).
For each stock, we can run a search on the hmm-dict that prints the isin, mic, instCode, uniqueId.
We can then run a search on the driver-dict that prints all the matches with the uniqueId and-or the uniqueId. We'll have to print the results in a readable way....
Remember that productInformation is made with hmm-dict

### algorithmics

python -d newScript.py test.csv

this will send an input contained in ../input/test.csv to the main program.

The main program only knows the list of IMS : qh-europe, asia, and consolidated, and the location for the IMS directories.

1 the input file is parsed into a list of str(isins). this is done with the modulereadinputfile module.

2 the dictionaries are searched for and their paths are put in a list. done with mri.MakeListOfDictionaryFolder

3 we now have a list of list and we flatten it to make a dictionary list.

4 the stocks are looped through:
in hmm-dict, we find all the matches for this isin.

## this is not fast.

At the moment we have a list of N stocks. each stock is compared to nearly 10 dictionaries, dictionaries have up to 100k stocks. so the loop time is extremely long.

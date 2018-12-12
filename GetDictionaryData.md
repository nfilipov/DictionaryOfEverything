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

# open the input file and make it a Python List

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

# read the dictionary files and parse their data into lists.

This one will have some xml in it. First we get to where the dict files are located.

This requires scanning the folders to skip the wrong ones and get only to the proper driver-dicts.

We'll once again rely on the powers of XML ElementTree python libs.

a getroot.attrib does the following:

```
{'mainIndex': 'instrumentCode', 'name': 'Dictionary', 'lotSize': '1'} # driver-dicts
{'mainIndex': 'uniqueId', 'capId': '328f4677', 'mddId': 'FUNCTION=2ee000', 'name': 'Dictionary'} # hmm-dicts
```
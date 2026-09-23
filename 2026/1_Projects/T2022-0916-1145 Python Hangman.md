---
tags: [state/closed/2022/9]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:30:49.137+02:00
---

# T2022-0916-1145 Python Hangman
#state/closed/2022/9
## Description
Create a hangman game in python.

## Todo

### Pseudocode
- [x] Open dictionary file
- [x] Pick a random German word
- [x] split the word into single characters
- (convert the characters to lower case)
- (Set the difficulty level (how many guesses are possible)
    - Based on the level set the max guess counter (german version typically 10-15)
    - Based on the level set the hang man design steps
- [x] Print the the place holders (number of characters with _ to the screen)
- [x] Create a list as long as the word with the placeholders _
- [x] Loop until user exits (input = 1)
    - [x] get user input: one character
    - (Input 2: Guess word)
        - User can input the word
        - If word is correct
            - Exit game > User won
        - if word is wrong
            - lower the hangman counter
            - add a part of the hangman to the display
            - If the hangman is finished
                - exit game > user has lost
    - [x] If character is in word (ignoring case)
        - Add the character selected by the user to a string, if it is not in the string already.
        - Print the character on the screen (replace the correct _ with the character)
        - if word is complete
            - Exit game > User won
    - [x] if character is not in word
        - lower the hangman counter
        - add a part of the hangman to the display
        - If the hangman is finished
            - exit game > user has lost

First create an simple version without hangman

### Diagram

### Howto
#### Preparation
##### Anaconda3
I don't yet really know how to use it.
Just created a new folder with the files.
##### Create Python venv ?
```bash
cd ~/Documents/python
mkdir hangman
```
##### Get dictinary list
The below command did look for the package that installed one of the currently existing dictionary files. I was searching for the file name. This helped to find out which package would install the german/swiss dictionary.
```python
dpkg -S british-english
wbritish: /usr/share/man/man5/british-english.5.gz
wbritish: /usr/share/dict/british-english
```

Install the Swiss and German dictionaries
```python
sudo apt install wngerman wswiss
```

The text word lists are located here
```python
ls -1 /usr/share/dict/{swiss,ngerman}
/usr/share/dict/ngerman
/usr/share/dict/swiss
```

### Open a file
Open the dictionary file
```python
file = open('/usr/share/dict/swiss','r')
words = file.readlines()
```

### Pick random German word
```python
import random
#...
words = file.read().splitlines()
print(random.choice(words))
```

### Split word
No need. You can address the single characters of a string like this.
```python
>>> print(word)
kalkulatorischem
# First char
>>> print(word[0])
k
# Last char
>>> print(word[len(word)-1])
m
```

### Print place holders
```python
# Save the place holder word with _ for each character
>>> placeholder="_" * int(len(word)-1)
# Print it with space separation
>>> print(*placeholder,sep=" ")
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _
```

### Create placeholder list
```python
placeholder="_" * int(len(word))
placeholderlist=list(placeholder)
```

### Get user input in a loop
```python
while usercharacter != "1":
    print(*placeholder,sep=" ")

```

## Done
### 2022-09-16
- Created Task

### 2022-09-21
Done
Not perfect but nice :)

- Get dictionary list from dict.cc. There it should be possible to remove plural words.
   [[dict_cc]]

- What happens if the file can not be opened?
    ```bash
    Traceback (most recent call last):
      File "hangman.py", line 97, in <module>
        word,placeholderlist=getrandomword()
      File "hangman.py", line 91, in getrandomword
        file = open('/usr/share/dict/swiss','r')
    FileNotFoundError: [Errno 2] No such file or directory: '/usr/share/dict/swiss'

    ```
    - Handle exceptions
```bash
def getrandomword():
    try:
        filename='/usr/share/dict/swiss'
        file = open(filename,'r')
    except OSError:
        print("Could not open/read file: ", filename)
        sys.exit()
    with file:
        wordlist = file.read().splitlines()
    word=random.choice(wordlist)
    placeholder="_" * int(len(word))
    placeholderlist=list(placeholder)
    return word, placeholderlist
```

- [x] remove abbreviation words - words that only contain capital letters from the word list
- [x] Remove very long words (for example set a difficulty level that only allows a certain word length)
Both problems can be solved with a regex like this
```python
>>> re.findall(r'\W(\w[a-z]{3,8})\W',wordlist)
['Aachen', 'Aachenern', 'Aachens', 'Aals', 'Aases', 'Aasgeiern', 'Abakus', 'Abart', 'Abbau', 'Abbaus', 'Abbiegens', 'Abbiegern', 'Abbiegung', 'Abbild']
```
- Starting with a non word character (\n)
- any word character
- and then a defined number of small letters (here 3 to 8)
- and then ending with a non word character (\n).

## Code
2022-09-21
```python
# Created 2022-09-21
# Version 1.0
# Author: ME

import random,sys,re

def printhangman(progress):
    global hangmancounter
    hangmanascii = [
    '''
        
        
        
        
        
        
    =========''', '''
        +
        |
        |
        |
        |
        |
    =========''', '''
    +---+
        |
        |
        |
        |
        |
    =========''', '''
    +---+
       \|
        |
        |
        |
        |
    =========''', '''
    +---+
    |  \|
        |
        |
        |
        |
    =========''', '''
    +---+
    |  \|
    O   |
        |
        |
        |
    =========''', '''
    +---+
    |  \|
    O   |
    |   |
        |
        |
    =========''', '''
    +---+
    |  \|
    O   |
   /|   |
        |
        |
    =========''', '''
    +---+
    |  \|
    O   |
   /|\  |
        |
        |
    =========''', '''
    +---+
    |  \|
    O   |
   /|\  |
   /    |
        |
    =========''', '''
    +---+
    |  \|
    O   |
   /|\  |
   / \  |
        |
    =========''']
    hangmancounter=len(hangmanascii)-1
    print(hangmanascii[progress])

def getrandomword():
    try:
        filename='/usr/share/dict/swiss'
        file = open(filename,'r')
    except OSError:
        print("Could not open/read file: ", filename)
        sys.exit()
    with file:
        # Number of characters the word should have (one at the beginneing +minimal/maximal range)
        minimal=3
        maximal=5
        wordlist = re.findall(r'\W(\w[a-z]{'+str(minimal)+','+str(maximal)+'})\W',file.read())
    word=random.choice(wordlist)
    placeholder="_" * int(len(word))
    placeholderlist=list(placeholder)
    return word, placeholderlist

word,placeholderlist=getrandomword()
#placeholder="_" * int(len(word))
#placeholderlist=list(placeholder)
usercharacter=""
charactercounter={}
nbofchar=0
failcounter=0
while usercharacter != "1":
    printhangman(failcounter)
    print("Already used characters: ","".join(charactercounter.keys()))
    print(*placeholderlist,sep=" ")
    print()
    usercharacter=input("Give me a character (a-z, lower case) find the word or enter 1 to exit: ")
    print(*placeholderlist,sep=" ")
    if usercharacter in word:
        if not charactercounter.get(usercharacter):
            charactercounter[usercharacter]=1
            # How many characters of the word have been guessed correctly
            nbofchar+=word.count(usercharacter)
        else:
            charactercounter[usercharacter]+=1
            failcounter+=1
    else:
        charactercounter[usercharacter]=0
        failcounter+=1
    print("\nFailed attempts: {}".format(failcounter))
    if nbofchar == len(word):
        print("You won. Congratulations")
        sys.exit()
    if failcounter == hangmancounter:
        print("\nYou lost. Try again!")
        print("Theword would have been: {}".format(word))
        sys.exit()
    else:
        printhangman(failcounter)
    for index,wordcharacter in enumerate(word):
        if wordcharacter.lower() == usercharacter:
            placeholderlist[index]=wordcharacter
    print()
```
- Both non word characters are cut out with the capture group

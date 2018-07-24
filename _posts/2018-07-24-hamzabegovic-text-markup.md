---
layout: post
title: Text Markup
image: /img/hello_world.jpeg
---
### Task:
For this task we had to clean our dispatch from tags etc. with a python script and create texts from each issue in seperate `.txt` files.
First my code did not really work, but with the help of some collegues and the published solution I managed to clean the dispatch. Finally the script
created over 22000 `.txt` files. I am not sure though if that is supposed to happen, but it seemed correct, since it splitted the articles from
advertises and other unnecessary stuff. I pretty much used the script, which was published as the solution.

### Script:
```
import re, os

source = "path_where_initial_files_are"
target = "path_to_save_new_files"

lof = os.listdir(source)
counter = 0 # general counter to keep track of the progress

for f in lof:
    if f.startswith("dltext"): # fileName test        
        with open(source + f, "r", encoding="utf8") as f1:
            text = f1.read()

            # try to find the date
            date = re.search(r'<date value="([\d-]+)"', text).group(1)

            # splitting the issue into articles/items
            split = re.split("<div3 ", text)

            c = 0 # item counter
            for s in split[1:]:
                c += 1
                s = "<div3 " + s # a step to restore the integrity of items
                #input(s)

                # try to find a unitType
                try:
                    unitType = re.search(r'type="([^\"]+)"', s).group(1)
                except:
                    unitType = "noType"
                    print(s)

                # try to find a header
                try:
                    header = re.search(r'<head>(.*)</head>', s).group(1)
                    header = re.sub("<[^<]+>", "", header)
                except:
                    header = "NO HEADER"
                    print("\nNo header found!\n")

                text = re.sub("<[^<]+>", "", s)
                text = re.sub(" +\n|\n +", "\n", text)
                text = re.sub("\n+", ";;; ", text)

                # generating necessary bits 
                fName = date+"_"+unitType+"_"+str(c)

                itemID = "#ID: " + date+"_"+unitType+"_"+str(c)
                dateVar   = "#DATE: " + date
                unitType = "#TYPE: " + unitType
                header = "#HEADER: " + header
                text = "#TEXT: " + text

                # creating a text variable
                var = "\n".join([itemID,dateVar,unitType,header,text])
                #input(var)

                # saving
                with open(target+fName+".txt", "w", encoding="utf8") as f9:
                    f9.write(var)

        # count processed issues and print progress counter at every 100        
        counter += 1
        if counter % 100 == 0:
            print(counter)
```

### Example:

```
#ID: 1860-11-10_article_21
#DATE: 1860-11-10
#TYPE: article
#HEADER: NO HEADER
#TEXT: ;;; Lampblack and Oil the cause of Fire.;;; --An English manufacturer states that one of his workmen placed a ladle, which had been recently used for the purpose of measuring linseed oil, upon the top of a cask of lampblack, and a few drops of the oil fell into the cask.;;; One evening, just before closing the works, he discovered a very disagreeable smell, and searched the factory to ascertain the cause, and, to his surprise, found the whole of the black in the cask resemble a large ball of fire, and there is no doubt that before morning it would have burst into a flame, and caused not only the destruction of the stock, but of the entire premises.;;; My plan has been (says the above manufacturer,) since the occurrence, not to keep more black in stock than is required for present use. Wood or canvas painted with lampblack and oil ought to be carefully looked to, at least until well seasoned.;;; 
```

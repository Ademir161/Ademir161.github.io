---
layout: post
title: Social Network Analysis
image: /img/hello_world.jpeg
---
### Task:
For this homework we had to create a network of people and places mentioned in the dispatch. First we had to collect names and typonyms
and remove duplicates. Then create edges by counting frequencies in a dictionary and visualize it with Gephi. Unfortunetly my code did
not really work, so Erika Unterpertinger provided us her code, but hers did not work either. I kept getting an empty list with three 
columns ``source target weight`` only.
### Code:
```
import re, os

source = os.path.abspath('source')
target = os.path.abspath('output')


edgesDic = {} 
edgesList = []

def updateDic(dic, key): 
	if key in dic:
		dic[key] += 1
	else:
		dic[key] = 1
	
def taggedNames(unitText):
	namesInUnit = [] 
	for names in re.findall(r'<[pP]ersName[^<]+', unitText, flags=0):
		match = re.search(r'authname="([\w,]+)"', names)
		if match:
			name = match.group(1) 													
			name = name.lower()
			namesInUnit.append(name)
	return namesInUnit

import itertools 
def edges(edgesList, edgesDic):
	edges = list(itertools.combinations(edgesList, 2))
	for e in edges:
		key = "\t".join(sorted(list(e)))
		updateDic(edgesDic, key)	

lof = os.listdir(source)
lof.sort()

for file in lof:
	with open(source + "/" + file, "r", encoding="utf8") as f1:
		text = f1.read()
			
		split = re.split('<div3', text)
		for unit in split[1:]:
			unit = "<div3" + unit		
			
			
			namesList = taggedNames(unit)		
			edges(namesList, edgesDic)
			


with open(target+"/"+"result.tsv", "w", encoding="utf8") as resultFile:
	resultFile.write("source\ttarget\tweight\n")
	for edge in edgesDic:
		entry = edge+"\t"+str(edgesDic[edge])+"\n"
		resultFile.write(entry)
```

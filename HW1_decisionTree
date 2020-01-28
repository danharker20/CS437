import math
import operator

attributes = ["color","type","origin"]
colorAttr = ["red","yellow"]
typeAttr = ["sports","suv"]
originAttr = ["domestic","imported"]
allAttrs = [colorAttr,typeAttr,originAttr]
data = {1:{"color":"red","type":"sports","origin":"domestic","stolen":"yes"},
        2:{"color":"red","type":"sports","origin":"domestic","stolen":"no"},
        3:{"color":"red","type":"sports","origin":"domestic","stolen":"yes"},
        4:{"color":"yellow","type":"sports","origin":"domestic","stolen":"no"},
        5:{"color":"yellow","sports":"sports","origin":"imported","stolen":"yes"},
        6:{"color":"yellow","type":"suv","origin":"imported","stolen":"no"},
        7:{"color":"yellow","type":"suv","origin":"imported","stolen":"yes"},
        8:{"color":"yellow","type":"suv","origin":"domestic","stolen":"no"},
        9:{"color":"red","type":"suv","origin":"imported","stolen":"no"},
        10:{"color":"red","type":"sports","origin":"imported","stolen":"yes"}}

class Node(object):
    def __init__(self, name, possibleAttrs = attributes, parent = None):
        self.name = name
        self.namesList = []
        self.possibleAttrs = possibleAttrs
        self.entropy = 0.0
        self.parent = parent
        self.left = None
        self.right = None
        self.stolen = None
    
    def set_entropy(self, entropy):
        self.entropy = entropy

    def set_children(self, left, right):
        self.left = left
        self.right = right

    def set_stolen(self, stolen):
        self.stolen = stolen

    def update_namesList(self, name):
        self.namesList.append(name)

    def update_possibleAttrs(self, attrToRemove):
        self.possibleAttrs.remove(attrToRemove)

    def reset_possibleAttrs(self):
        self.possibleAttrs = ["color","type","origin"]

# CALCULATE ENTROPY
def calculateEntropy(entropyAttributes):
  numYes = 0
  numNo = 0
  i=0
  temp=0
  goodids=[]

  # IF NO VALUES IN entropyAttributes
  if (len(entropyAttributes) == 0):
    for id in data:
      if (data[id]["stolen"] == "yes"):
        numYes+=1
      else:
        numNo+=1
      i+=1
  # IF VALUES IN entropyAttributes
  else:
    for id in data:
      for attr in entropyAttributes:
        if (attr in data[id].values()):
          temp += 1
      if (temp == len(entropyAttributes)):
        #print("ID: ", id) #just a check
        if (data[id]["stolen"] == "yes"):
          numYes += 1
        else:
          numNo += 1  
        i += 1 
      temp = 0   

  # CALCULATE ENTROPY
  #print("[",numYes,"+,",numNo,"-]")
  return [round((-(numYes/i)*math.log((numYes/i),2)) - ((numNo/i)*math.log((numNo/i),2)),3), numYes, numNo, i] #the actual entropy calculation

# CALCULATE GAIN
def calculateGain(entropyAttributes, gainAttributes):
    entropy       = calculateEntropy(entropyAttributes)[0]
    entropy_total = calculateEntropy(entropyAttributes)[3]

    for attr in gainAttributes:
        curEntropy = calculateEntropy([attr])[0]
        curTotal   = calculateEntropy([attr])[3]
        
        entropy -= (curTotal/entropy_total) * curEntropy
    
    return round(entropy,3)

def printInfo(curNode, leftChild, rightChild):
    # PRINT INFO
    print("\nCUR NAME:          " + str(curNode.name))
    print("CUR NAMES LIST:    " + str(curNode.namesList))
    print("CUR POSS ATTRS:    " + str(curNode.possibleAttrs))
    print("CUR ENTROPY:       " + str(curNode.entropy))
    if (curNode.name != "root"):
        print("CUR PARENT NAME:   " + str(curNode.parent.name))
    else:
        print("CUR PARENT NAME:   DOES NOT EXIST")
    print("CUR LEFT PTR:      " + str(curNode.left.name))
    print("CUR RIGHT PTR:     " + str(curNode.right.name))
    print("CUR STOLEN:        " + str(curNode.stolen))

    print("\nLEFT NAME:         " + str(leftChild.name))
    print("LEFT NAMES LIST:   " + str(leftChild.namesList))
    print("LEFT POSS ATTRS:   " + str(leftChild.possibleAttrs))
    print("LEFT ENTROPY:      " + str(leftChild.entropy))
    print("LEFT PARENT NAME:  " + str(leftChild.parent.name))
    if (leftChild.left == None):
        print("LEFT LEFT PTR:     " + str(leftChild.left))
    else:
        print("LEFT LEFT PTR:     " + str(leftChild.left.name))
    if (rightChild.right == None):
        print("LEFT RIGHT PTR:    " + str(rightChild.right))
    else:
        print("LEFT RIGHT PTR:    " + str(rightChild.right.name))
    print("LEFT STOLEN:       " + str(leftChild.stolen))
    
    print("\nRIGHT NAME:        " + str(rightChild.name))
    print("RIGHT NAMES LIST:  " + str(rightChild.namesList))
    print("RIGHT POSS ATTRS:  " + str(rightChild.possibleAttrs))
    print("RIGHT ENTROPY:     " + str(rightChild.entropy))
    print("RIGHT PARENT NAME: " + str(rightChild.parent.name))
    print("RIGHT LEFT PTR:    " + str(rightChild.left))
    print("RIGHT RIGHT PTR:   " + str(rightChild.right))
    print("RIGHT STOLEN:      " + str(rightChild.stolen))
    
    print("\n************************************************************************")

def createTree(curNode, root):

    gainValues = {}

    # CHECK IF DONE
    if (curNode.possibleAttrs == []):
        root.reset_possibleAttrs()
        # print(root.possibleAttrs)
        return

    # SET ENTROPY FOR curNode
    curNode.set_entropy(calculateEntropy(curNode.namesList)[0])

    # DETERMINE WHICH ATTRIBUTE TO USE FOR CHILDREN
    # GET ALL POSSIBLE ATTRIBUTES
    for attr in curNode.possibleAttrs:
        if (attr == "color"):
            gainValues[attr] = calculateGain(curNode.namesList, colorAttr)
        elif (attr == "type"):
            gainValues[attr] = calculateGain(curNode.namesList, typeAttr)
        else:
            gainValues[attr] = calculateGain(curNode.namesList, originAttr)
 
    # GET ATTRIBUTE BY MAX VALUE
    gainMaxAttr = max(gainValues.items(), key=operator.itemgetter(1))[0] 
    #print("Max attr: " + gainMaxAttr + "\nMax value: " + str(gainValues[gainMaxAttr]))

    # REMOVE ATTRIBUTE FROM possibleAttrs
    curNode.update_possibleAttrs(gainMaxAttr)
    #print("Remaining attrs: " + str(curNode.possibleAttrs))

    #############################################################################
    # leftChild  = Node(colorAttr[0], curNode.possibleAttrs, curNode)     #init w/ name, possibleAttrs, parent
    # rightChild = Node(colorAttr[1], curNode.possibleAttrs, curNode)     #init w/ name, possibleAttrs, parent
        
    # SET CHILDREN BASED ON gainMaxAttr
    # INIT CHILDREN
    if (gainMaxAttr == "color"):
        if (curNode.left == None):
            curNode.left = Node(colorAttr[0], curNode.possibleAttrs, curNode)
            
        else:
            createTree(curNode.left, root)

        if (curNode.right == None):
            curNode.right = Node(colorAttr[1], curNode.possibleAttrs, curNode)
        else:
            createTree(curNode.right, root)     

    elif (gainMaxAttr == "type"):
        if (curNode.left == None):
            curNode.left = Node(typeAttr[0], curNode.possibleAttrs, curNode)
        else:
            createTree(curNode.left, root)

        if (curNode.right == None):
            curNode.right = Node(typeAttr[1], curNode.possibleAttrs, curNode)
        else:
            createTree(curNode.right, root)   

    else:
        if (curNode.left == None):
            curNode.left = Node(originAttr[0], curNode.possibleAttrs, curNode)
        else:
            createTree(curNode.left, root)

        if (curNode.right == None):
            curNode.right = Node(originAttr[1], curNode.possibleAttrs, curNode)
        else:
            createTree(curNode.right, root)   
   
    # LINK CHILDREN TO curNode
    # curNode.left  = leftChild
    # curNode.right = rightChild

    # UPDATE CHILDREN namesList (APPEND NAME USED)
    curNode.left.update_namesList(curNode.left.name)
    curNode.right.update_namesList(curNode.right.name)

    # SET CHILDREN ENTROPY
    curNode.left.set_entropy(calculateEntropy(curNode.left.namesList)[0])
    curNode.right.set_entropy(calculateEntropy(curNode.right.namesList)[0])
        
    # PRINT INFO
    printInfo(curNode, curNode.left, curNode.right)

    # ADD ALL CHILDREN
    # curNode.left.left = createTree(curNode.left, root)
    # curNode.left.right = createTree(curNode.left, root)

    # curNode.right.left = createTree(curNode.right, root)
    # curNode.right.right = createTree(curNode.right, root)

    # OLD STUFF
    # if (side == "left"):
    #     leftChild.left = createTree(leftChild, root, side)
    #     leftChild.right = createTree(leftChild, root, side)

    # if (side == "right"):
    #     rightChild.left = createTree(rightChild, root, side)
    #     rightChild.right = createTree(rightChild, root, side)
    
def printTree(curNode):
    if (curNode == None):
        return
        
    print(curNode.name)
    printTree(curNode.left)    
    printTree(curNode.right)
    

def main():
    
    #initialize root
    root = Node("root")
    print("\n")
    #print(root.name, root.entropy)

    #create tree starting from root
    createTree(root, root)
    createTree(root, root)
        
    printTree(root)
    #check children
    # print(root.left.stolen)
    # print(root.right.stolen)

main()

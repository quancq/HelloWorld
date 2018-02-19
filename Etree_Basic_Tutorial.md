
# <span style='color:green'>Etree Tutorial</span>


```python
from lxml import etree
```

## <span style='color:green'>1. Create/Remove/Insert Element</span>

### <span style='color:green'>1.1. **Create Element**</span>


```python
# Create Element/Node in XML tree structure 
root = etree.Element('html')
```


```python
# Create child element and append to parent element
head_elm = etree.SubElement(root, 'head')
```


```python
# Create element with attributes
body_elm = etree.SubElement(root, 'body', width='100px', height='200px')
```

### <span style='color:green'>1.2. **Insert Element**</span>


```python
# Insert child element into list children
root.insert(index=1, element=etree.Element('div', id='div1'))
root.insert(index=len(root), element=etree.Element('temp'))
```


```python
# Append child element into last of list children
head_elm.append(etree.Element('script'))
div_elm2 = etree.SubElement(body_elm, 'div', id='div2')
h1_elm = etree.SubElement(div_elm2, 'h1')
h1_elm.text = 'Text in h1'
h1_elm.tail = 'Tail in h1'
etree.SubElement(div_elm2, 'h2').text = 'Text in h2'
```


```python
# Add sibling element
div_elm2.addprevious(etree.Element('h1'))
div_elm2.addnext(etree.Element('div'))
```

### <span style='color:green'>1.3. **Remove Element**</span>


```python
# Slice element
print(root[-1].tag)
# Remove element
root.remove(root[-1])
```

    temp
    


```python
# Clear element
temp = etree.Element('root')
etree.SubElement(temp, 'a').text = "Text in a"
b_temp = etree.SubElement(temp, 'b')
b_temp.append(etree.Element('b_child'))
etree.SubElement(temp, 'c').tail = "Tail in c"

print(etree.tostring(temp))
temp.clear()
print(etree.tostring(temp))
```

    b'<root><a>Text in a</a><b><b_child/></b><c/>Tail in c</root>'
    b'<root/>'
    

## <span style='color:green'>2. **Print pretty elements**</span>


```python
# print pretty (indent) html/xml document
print(etree.tostring(root, pretty_print=True).decode('utf-8'))
```

    <html>
      <head>
        <script/>
      </head>
      <div id="div1"/>
      <body height="200px" width="100px">
        <h1/>
        <div id="div2"><h1>Text in h1</h1>Tail in h1<h2>Text in h2</h2></div>
        <div/>
      </body>
    </html>
    
    

## <span style='color:green'>3. **Write/Parse xml document**</span>

### <span style='color:green'>3.1. **Write Elements to file**</span>


```python
# ElementTree wrapper
etree.ElementTree(root).write('simple_xml.xhtml')
```

### <span style='color:green'>3.2. **Parse Elements from string/file**</span>


```python
# parse from string
div_str = "<div><h2>Lxml</h2></div>"
temp1 = etree.fromstring(div_str)
# The XML() function behaves like the fromstring() function, but is commonly used to write XML literals right into the source
temp2 = etree.XML("<div><h2>Lxml</h2></div>")
print(etree.tostring(temp1, pretty_print=True).decode('utf-8'))
print(etree.tostring(temp2, pretty_print=True).decode('utf-8'))
```

    <div>
      <h2>Lxml</h2>
    </div>
    
    <div>
      <h2>Lxml</h2>
    </div>
    
    


```python
# parse from file
tree = etree.parse('simple_xml.xhtml')
root = tree.getroot()
# print pretty document
print(etree.tostring(root, pretty_print=True).decode('utf-8'))
```

    <html>
      <head>
        <script/>
      </head>
      <div id="div1"/>
      <body height="200px" width="100px">
        <h1/>
        <div id="div2"><h1>Text in h1</h1>Tail in h1<h2>Text in h2</h2></div>
        <div/>
      </body>
    </html>
    
    

## <span style='color:green'>4. **Traversal elements**</span>

### <span style='color:green'>4.1. **Traversal direct subelement**</span>


```python
for child in root:
    print(child.tag)
```

    head
    div
    body
    


```python
for child in root.iterchildren(reversed=True):
    print(child.tag)
```

    body
    div
    head
    

### <span style='color:green'>4.2. **Traversal all-level subelement**</span>


```python
for sibling in root[1].itersiblings(preceding=True):
    print(sibling.tag)
```

    head
    


```python
elm = root[2][1][0]
print(etree.tostring(elm).decode('utf-8'))
for ancestor in elm.iterancestors():
    print(ancestor.tag)
```

    <h1>Text in h1</h1>Tail in h1
    div
    body
    html
    


```python
for descendant in root.iterdescendants('h1', 'div'):
    print(descendant.tag)
```

    div
    h1
    div
    h1
    div
    

## <span style='color:green'>5. **Find Elements**</span>

### <span style='color:green'>5.1. **Find all-level subelement**</span>


```python
#  If a tag name is given, only first-level subelements are checked. Path expressions can be used to search the entire subtree.
```


```python
elm = root.find('body')
print(etree.tostring(elm))
if root.find('h1') is None:
    print("Nothing h1 element in first level of root")
```

    b'<body height="200px" width="100px"><h1/><div id="div2"><h1>Text in h1</h1>Tail in h1<h2>Text in h2</h2></div><div/></body>'
    Nothing h1 element in first level of root
    


```python
text = root.findtext('h1')
print(text)
text = root.findtext('h1', default='OK')
print(text)
```

    None
    OK
    


```python
elms = root.findall('body')
print([etree.tostring(elm) for elm in elms])
elms = root.findall('h1')
print([etree.tostring(elm) for elm in elms])
```

    [b'<body height="200px" width="100px"><h1/><div id="div2"><h1>Text in h1</h1>Tail in h1<h2>Text in h2</h2></div><div/></body>']
    []
    

### <span style='color:green'>5.2. **Find Relative Element**</span>


```python
# Get parent
print(etree.tostring(div_elm2.getparent(), pretty_print=True).decode('utf-8'))
```

    <body height="200px" width="100px">
      <h1/>
      <div id="div2"><h1>Text in h1</h1>Tail in h1<h2>Text in h2</h2></div>
      <div/>
    </body>
    
    


```python
# Get siblings
print(etree.tostring(div_elm2.getprevious()))
print(etree.tostring(div_elm2.getnext()))
```

    b'<h1/>'
    b'<div/>'
    


```python
# Get root
print(etree.tostring(div_elm2.getroottree().getroot(), pretty_print=True).decode('utf-8'))
```

    <html>
      <head>
        <script/>
      </head>
      <div id="div1"/>
      <body height="200px" width="100px">
        <h1/>
        <div id="div2"><h1>Text in h1</h1>Tail in h1<h2>Text in h2</h2></div>
        <div/>
      </body>
    </html>
    
    

## <span style='color:green'>6. **Other Property/Method of Element Object**</span>

### <span style='color:green'>6.1. **Property Element**</span>


```python
# Add text
body_elm[0].text = "Hello QuanCQ"
print(etree.tostring(body_elm, pretty_print=True, xml_declaration=True).decode('utf-8'))
```

    <?xml version='1.0' encoding='ASCII'?>
    <body height="200px" width="100px">
      <h1>Hello QuanCQ</h1>
      <div id="div2"><h1>Text in h1</h1>Tail in h1<h2>Text in h2</h2></div>
      <div/>
    </body>
    
    


```python
# Add tail
body_elm[0].tail = "Hihi"
print(etree.tostring(body_elm, pretty_print=True, xml_declaration=True).decode('utf-8'))
```

    <?xml version='1.0' encoding='ASCII'?>
    <body height="200px" width="100px"><h1>Hello QuanCQ</h1>Hihi<div id="div2"><h1>Text in h1</h1>Tail in h1<h2>Text in h2</h2></div><div/></body>
    
    


```python
# Atrib property
print(body_elm.attrib)
```

    {'height': '200px', 'width': '100px'}
    

### <span style='color:green'>6.2. **Method Element**</span>


```python
# check whether element has children
if len(root):
    print("Number children of root is ", len(root))
```

    Number children of root is  3
    


```python
# get
print(body_elm.get('width'))
print(body_elm.get('bgcolor', 'green'))
```

    100px
    green
    


```python
# set
print(body_elm.set('bgcolor', 'green'))
print(body_elm.attrib)
```

    None
    {'height': '200px', 'width': '100px', 'bgcolor': 'green'}
    


```python
# keys
print(body_elm.keys())
```

    ['height', 'width', 'bgcolor']
    


```python
# items
for (k,v) in body_elm.items():
    print("key = {}, value = {}".format(k,v))
```

    key = height, value = 200px
    key = width, value = 100px
    key = bgcolor, value = green
    

# <span style='color:green'>References</span>

### 1. [http://effbot.org/zone/pythondoc-elementtree-ElementTree.htm](http://effbot.org/zone/pythondoc-elementtree-ElementTree.htm)

### 2. [http://effbot.org/zone/element.htm](http://effbot.org/zone/element.htm)

### 3. [http://effbot.org/zone/element-xpath.htm](http://effbot.org/zone/element-xpath.htm)

### 4. [http://lxml.de/tutorial.html](http://lxml.de/tutorial.html)

### 5. [http://lxml.de/api.html](http://lxml.de/api.html)



```python
from lxml import etree
```

# Heading1

## <span style='color:green'>1. Create/Remove/Insert Element</span>

### <span style='color:green'>1.1. **Create Element**</span>


```python
# Create Element/Node in XML tree structure 
root = etree.Element('html')
```


```python
# Create child element and append to parent element
head_elm = etree.SubElement(root, 'head')
head_elm.append(etree.Element('script'))
body_elm = etree.SubElement(root, 'body')
```

### <span style='color:green'>1.2. **Insert Element**</span>


```python
# Insert child element into list children
root.insert(index=1, element=etree.Element('div'))
```

### <span style='color:green'>1.3. **Remove Element**</span>


```python
# print pretty (indent) html/xml document
print(etree.tostring(root, pretty_print=True).decode('utf-8'))
```

    <html>
      <head>
        <script/>
      </head>
      <div/>
      <body/>
    </html>
    
    


```python
# loop through list children of element
for child in root:
    print(child.tag)
```

    head
    div
    body
    


```python
# check whether element has children
if len(root):
    print("Number children of root is ", len(root))
```

    Number children of root is  3
    


```python
# add attributes to element
body_elm.append(etree.Element('div', width='50px', height='60px'))
```


```python
# get/set attributes
div_elm = body_elm[0]
div_elm.set('bgcolor', 'green')
print(div_elm.get('bgcolor'))
print(etree.tostring(div_elm, pretty_print=True).decode('utf-8'))
```

    green
    <div height="60px" width="50px" bgcolor="green"/>
    
    


```python
# add text to element
etree.SubElement(div_elm, 'h1').text = "Hello QuanCQ"
print(etree.tostring(div_elm, pretty_print=True, xml_declaration=True).decode('utf-8'))
```

    <?xml version='1.0' encoding='ASCII'?>
    <div height="60px" width="50px" bgcolor="green">
      <h1>Hello QuanCQ</h1>
    </div>
    
    


```python
# parse xml string to element
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
    
    

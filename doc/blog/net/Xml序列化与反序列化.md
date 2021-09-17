## Xml序列化与反序列化

---

```c#
　　　　　一、将xml虚化化成对象
        　　StringReader strReader = new StringReader(result);
            XmlReader xmlReader = XmlReader.Create(strReader);
            XmlSerializer serializer = new XmlSerializer(typeof(line_home));
            line_home dt = serializer.Deserialize(xmlReader) as line_home;

　　　　二、将对象序列化和成xml
　　　　  using (System.IO.FileStream stream = new System.IO.FileStream(fileName, System.IO.FileMode.Create))  
　　　　　{       
  　　　　　　System.Xml.Serialization.XmlSerializer serializer = new System.Xml.Serialization.XmlSerializer(entityObj.GetType());    
     　　　　serializer.Serialize(stream, entityObj);    
  　　　　} 


            StringReader strReader = new StringReader(result);
            XmlReader xmlReader = XmlReader.Create(strReader);
            XmlSerializer serializer = new XmlSerializer(typeof(line_home));
            line_home dt = serializer.Deserialize(xmlReader) as line_home;
```


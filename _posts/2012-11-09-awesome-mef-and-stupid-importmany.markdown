---
author: admin
comments: false
date: 2012-11-09 03:32:13+00:00
layout: post
slug: awesome-mef-and-stupid-importmany
title: Awesome MEF and Stupid ImportMany
wordpress_id: 100
categories:
- programming
tags:
- c#
- code
- linq
- mef
---

I absolutely love [Managed Extensibility Framework (MEF)](http://mef.codeplex.com/), I do. Most of the times I use MEF only for simple `Imports` and `Exports`. Yesterday, I was trying make some advanced use of it - decorate the classes with [Metadata](http://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.metadatatypeattribute.aspx) so that I can have a collection of the classes based on some custom attributes. Oh, Boy! I struggled a lot and end up spending couple of hours making some stupid mistakes. The official documentation was not of much help, and so was StackOverflow archives. So, I ended up figuring out everything on my own with some hits and trials. Here is a small example demonstrating how to use `ImportMany` with strongly typed custom attribute.





First the interface that each of the exported classes is going to implement:




    
    public interface ISerializer {
         void Serialize(string path, string[] data);
    }
    





And an enum that defines the type of a serializer.




    
    public enum SerializerType{
         Xml,
         Binary,
         Yaml,
    }
    





Now the metadata part. To be able to access metadata in a strongly typed way, you need to define an interface that matches **JUST** the name of the properties you are trying to access:




    
    public interface ISerializerMetadata{
         /* It is very important to make it an array/ collection type 
            because we will allow a class to have multiple attributes. 
            This is where I screwed up.*/
    
         SerializerType[] ItsSerializerTypes { get; }
    }
    





Now, the actual attribute:




    
    [MetadataAttribute]
    [AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
    public class SerializerAttribute : ExportAttribute{
         public SerializerAttribute(SerializerType type) : base(typeof(ISerializer)){
         }
    
         /* Now the property that will be used to find the type of 
            each class. Remember the name HAS to match with that of 
            the name of the property defined above in ISerializerMetadata 
            interface. 
            Also notice the type - SerializerType, which is different than the 
            one from ISerializerMetadata interface. ONLY THE NAME SHOULD MATCH. */
    
           public SerializerType ItsSerializerTypes { get; }
    }
    





Let's write a class that can serialize both Xml and Yaml (for the sake of this example):




    
    [Serializer(SerializerType.Xml)]
    [Serializer(SerializerType.Yaml)]
    public class AnyMarkupSerializer : ISerializer{
          public void Serialize(string path, string[] data) {
               // left out the implementation
          }
    }
    





That's all you need to export a class that has been marked as a class that can, supposedly, serialize an array of string to xml as well as yaml format.





Now, let's have a `DocumentController` class which wants to import all the exported serializers.




    
    [Export]
    public class DocumentController {
        [ImportMany]
        public IEnumerable<Lazy<ISerializer, ISerializerMetadata>> ItsAllSerializers { get; set; }
        
    
        private IEnumerable<ISerializer> ItsMarkupSerializers { get; set; }
    
        public DocumentController(){
            // This can go anywhere, I'm putting inside the constructor for simplicity.
            // Also, you probably won't need multiple xml serializers in real life but, again, this is just an example
            var xmlSerializers = ItsAllSerializers.Where(e => e.Metadata.ItsSerializerTypes.Contains(SerializerType.Xml))
                                                  .Select(e=>e.Value).Distinct().ToList();
            var yamlSerializers = ItsAllSerializers.Where(e => e.Metadata.ItsSerializerTypes.Contains(SerializerType.Yaml))
                                                   .Select(e=>e.Value).Distinct().ToList();
    
           ...   
        }
    }
    




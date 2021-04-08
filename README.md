# .NET Core Mocking Request.Form for Unit Testing

We use form-data s a lot to transfer files and various types of datas while working with APIs and web requests. In .Net Core these files can be accessed via Request.Form object in controllers. These files basically are stored here as encoded bytes to StringValues type. Normally what we need to do use them is passing them to a IFormFile variable and writing the bytes with the help of streams to files or use them accordingly. Below some examples will be shown about how to mock these form data to test our methods while unit testing.

# Mocking Files

Normally we can mock our data with creating classes with the contents we need. But since these files or data are coming within our Http request we follow a different way here. Request.Form is a property of our controller's HttpContext. So we need to create a new FormCollection object to put it into Request.Form property. First we need a file and its bytes in an array. In case your method checks for the type or doing some type-spesific work you need to be careful about bytes. Because **first couple of bytes of the file defines its file type**. Below I put a link to a very useful repository that includes the smallest files of most-used file types.
![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/setformdata.png)

In the method I wrote above a gif file is mocked and put in the Request.Form. After getting bytes of the file we need to pass it to a **MemoryStream** to be able to write it as a file. Here mime type is very important for file to be recognized. Also we need to say it is a **form-data** in headers. Then we are constructing our FormFile using the header and the stream we created. In the constructor of FormFile you can give the file name  and name attributes as parameters. I passed an empty string for both of them since I dont use them in my method. After that we create an FormFileCollection instance and add out FormFile to it. As the final step we pass tihs form in an FormCollection to our Request.Form.

![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/setformdata2.png)

Here is another version of this method which is to mock a Request.Form more data other than File. Basicaly we do the same thing for File part. You can add or remove parameters according to your needs. What it does here is serializing an object coming as a parameter and converts it  to **StringValues** type. We are adding it to our dictionary giving the **name which we will access the data with** as key. Because the dictionary we'll pass to FormCollection object must have **StringValues** values.


## Using the Mock Form File

![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/controller1.png)
In your test methods you can simply call the method before you call the actual method in project. Here you can also empty the formdata with giving a boolean parameter in case you may need to use non-Null, but  empty Request.Form. Here is another example where we use the second method with an extra parameter besides FormFile:
![enter image description here](https://raw.githubusercontent.com/berkevaroll/mock-requestform-dotnetcore/main/controller2.png)
Here we pass the object that we prepared as a theory. And it i added to Request.Form with key we gave in dictionary. That's pretty much all to do to mock files in Request.Form. We can also manipulate other properties and play around with Request to make it fit. Thanks for reading and hope that'll help your work. 

# Links

 - [Smallest possible [...] files](https://github.com/mathiasbynens/small)
 

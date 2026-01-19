- The `File` class from the `java.io` package
```java
import java.io.File;  // Import the File class

File myObj = new File("filename.txt"); // Specify the filename
```
## Useful methods from `File` class
- `Boolean canRead()` - tests whether the file is readable or not
- `Boolean canWrite()` - writable or not
- `Boolean createNewFile()` - create an empty file
- `Boolean delete()` - delete a file
- `Boolean exists()` - tests whether the file exists
- `String getName()` - returns the name of the file
- `String getAbsolutePath()` - returns the absolute pathname of the file
- `long Length()` - returns the size of the file in bytes
- `String[] list()` - returns an array of file in the directory
- `Boolean mkdir()` - creates a directory

## Create a file
```java
import java.io.File;  // Import the File class
import java.io.IOException;  // Import the IOException class to handle errors

public class CreateFile {
  public static void main(String[] args) {
    try {
      File myObj = new File("filename.txt");
      if (myObj.createNewFile()) {
        System.out.println("File created: " + myObj.getName());
      } else {
        System.out.println("File already exists.");
      }
    } catch (IOException e) {
      System.out.println("An error occurred.");
      e.printStackTrace();
    }
  }
}
```

## Write to a File
- use the `FileWriter` class with `write()` method to write some text to the file we created.
- When you are done writing to the line, you should close it with the `close()` method
```java
import java.io.FileWriter;   // Import the FileWriter class
import java.io.IOException;  // Import the IOException class to handle errors

public class WriteToFile {
  public static void main(String[] args) {
    try {
      FileWriter myWriter = new FileWriter("filename.txt");
      myWriter.write("Files in Java might be tricky, but it is fun enough!");
      myWriter.close();
      System.out.println("Successfully wrote to the file.");
    } catch (IOException e) {
      System.out.println("An error occurred.");
      e.printStackTrace();
    }
  }
}
```

## Read a File
- use the `Scanner` class to read the contents of the text file
```java
File myObj = new File("filename.txt");
Scanner myReader = new Scanner(myObj);
while (myReader.hasNextLine()) {
String data = myReader.nextLine();
System.out.println(data);
```

### Get File Information
```java
File myObj = new File("filename.txt");
if (myObj.exists()) {
  System.out.println("File name: " + myObj.getName());
  System.out.println("Absolute path: " + myObj.getAbsolutePath());
  System.out.println("Writeable: " + myObj.canWrite());
  System.out.println("Readable " + myObj.canRead());
  System.out.println("File size in bytes " + myObj.length());
} else {
  System.out.println("The file does not exist.");
}
```

## Delete files
### Delete a file
```java
File myObj = new File("filename.txt"); 
if (myObj.delete()) { 
  System.out.println("Deleted the file: " + myObj.getName());
} else {
  System.out.println("Failed to delete the file.");
} 
```

### Delete a folder
```java
File myObj = new File("C:\\Users\\MyName\\Test"); 
if (myObj.delete()) { 
  System.out.println("Deleted the folder: " + myObj.getName());
} else {
  System.out.println("Failed to delete the folder.");
} 
```
- ⚠️folder must be empty


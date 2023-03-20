# FileInputStream and FileOutputStream

## Learning Goals

- Learn how to write a file using the `FileOutputStream` class.
- Learn how to read a file using the `FileInputStream` class.

## Introduction

Another way to read and write files in Java is through the use of streams. We
have learned about streams previously when it comes to collections and arrays.
In this lesson, we'll learn about how we can apply those same concepts to
working with files.

## FileOutputStream

The `FileOutputStream` class provides an `OutputStream` for writing data out to
a file. We can construct a `FileOutputStream` by passing in a `File` object or
a `String` object with the file name.

```java
import java.io.File;
import java.io.FileOutputStream;

public class FileIOMain {
    public static void main(String[] args) {
        File file = new File("simple.txt");

        FileOutputStream fileOutputStream = new FileOutputStream(file);
    }
}
```

Now that we have our `FileOutputStream` instance instantiated, we can use this
to write out data to a file! It should be noted that `FileOutputStream` is meant
for writing streams of raw bytes out to a file. The Java Oracle documents
suggest using this class for writing out imaging data rather than streams of
characters. Let's look at why this might be the case:

```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileIOMain {
    public static void main(String[] args) {
        File file = new File("simple.txt");
        String output = "example of writing to a file";

         try (FileOutputStream fileOutputStream = new FileOutputStream(file)) {
            fileOutputStream.write(output.getBytes());
         } catch (IOException ioException) {
            ioException.printStackTrace();
         }
    }
}
```

As we saw before with the `BufferedWriter`, the `FileOutputStream` constructors
could throw an `IOException`, so we will need to wrap it in a try-with-resources
statement.

We also see that the `FileOutputStream` has a `write()` method too! But this
`write()` method takes some different parameters than the other `write()`
methods that we have seen so far. Instead of taking in a `String` or array of
characters, this `write` method takes in an array of bytes to be written out to
a file. Therefore, if we want to write characters out to a file, we need to
convert it to a `byte` array first. We can convert our `String` to a `byte`
array by using the `getBytes()` instance method. This is why it is suggested to
use the `FileOutputStream` class when writing data, such as images or binary
data, rather than writing streams of characters. For streams of characters, it
is recommended to use the `FileWriter` and `BufferedWriter` class still.

To show that we could still write characters out to a file though, let's run the
program above and see what happens:

![simple-text-file-content](https://curriculum-content.s3.amazonaws.com/java-mod-3/file-input-output/write-to-file-content.png)

The above shows that we can still write characters to `simple.txt` using the
`FileOutputStream` class.

## FileInputStream

`FileInputStream` is another class we can use to read the contents from a file.
Since it is a subclass of the `InputStream` class, the `FileInputStream` will
inherit all the functionality from the `InputStream`, plus added functionality
of being able to read from a file. We can construct a `FileInputStream` by
passing in a `File` object or a `String` object with the file name, like so:

```java
import java.io.File;
import java.io.FileInputStream;

public class FileIOMain {
    public static void main(String[] args) {
        File file = new File("simple.txt");

        FileInputStream fileInputStream = new FileInputStream(file);        
    }
}
```

Now that we have our `FileInputStream` instance instantiated, we can use this
to read the file, `simple.txt`. It should be noted that much like the
`FileOutputStream` class is meant for writing streams of raw bytes out to a
file, the `FileInputStream` is also meant for reading streams of raw bytes from
a file. The Java Oracle documents suggest using this class for reading in data,
like image data, rather than streams of characters. Let's look at how to use
the `FileInputStream` instance:

```java
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FileIOMain {
    public static void main(String[] args) {
        File file = new File("simple.txt");

        try (FileInputStream fileInputStream = new FileInputStream(file)) {
            int input = fileInputStream.read();

            // fileInputStream.read() will return -1 when it has reached the end of the file (EOF)
            while (input != -1) {

                System.out.print((char) input);

                // While not EOF, read the next byte from the file
                input = fileInputStream.read();
            }
        } catch (IOException ioException) {
            ioException.printStackTrace();
        }
    }
}
```

Lets breakdown this code:

- Just like the `FileOutputStream` class, we will need to wrap the
  `FileInputStream` instantiation in a try-with-resources statement.
- The `read` method here will read in a byte of data at a time from the input
  stream; therefore, we'll store each input byte as an `int` for us to cast to
  as a `char`.
- While we haven't reached the end of the file yet, which is signaled by a -1,
  we'll continue reading each byte. 

Since the `FileInputStream` reads in bytes rather than characters, it is
recommended to use the `FileInputStream` when reading in data, like images and
other binary data. For reading streams of characters, the documentation says to
consider using `FileReader` and `BufferedReader`.

If we run the code above, we'll see that the output is exactly what we
originally wrote to the file:

```text
example of writing to a file.
```

## Resources

- [Java 11 FileOutputStream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileOutputStream.html)
- [Java 11 FileInputStream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileInputStream.html)

# Networked System
# Server-Client Python Implementation 

## By George Flouris and Christos Stylianou

In this project we implement a networked application in which a server python script and a client python script, using the Python socket library, form a file service application.
The file service application works as expected. It can transfer data in binary thus successfully exchange any type of file. 
The server and client can handle all three requests specified; uploading a file (put), downloading a file (get) and listing requests.
The networked application works as follows:

## Put:

We opted to send the request type and filename as a string with delimiter "\0" because it cannot
be included in any filenames.
The client sends a message containing the following fields in order:
- the size of the message that includes the command, filename and file size
- the string "put" followed by a "\0" character and the filename followed by a “\0” and the
file_size as a string plus a "\0" character, all encoded to binary as a utf-8 string.
- the file

It then prints “file has been uploaded”.
The server receives 24 bytes of data (size of int in python) from the socket and decodes it as a utf-8
string and then to int. It then receives the next [x] number of bytes as indicated by the message it
just received. Then it decodes it to a string and splits (into command, filename and size) whenever
it finds a “\0”.
It then checks if the file exists with the existingfile function.
If the string equals "put" it points it to the relative function that keeps receiving file_size data and
writes it in a file with the filename it previously decoded and prints “file has been uploaded with
the name and file_size”.

## Existingfile:
Takes the filename and opens a file with that name. If it exists it raises FILEEXISTSERROR. If file not
exists error is raised, we catch it and return false as there is no existing file.

## Recv_start:
Takes the header and splits it, it then calls the relative function to the command.

## Get_header_size:
Takes the header and returns its binary size.

## Send_header_size:
Sends the header size.

## Recv_header_size:
Receives the header size.

## Open_file:
Takes the filename and returns the file in binary and file size.

## Recv_all:
It takes the socket and size of data to receive as parameter and then loops until it gets size
amount of data. We opted to use a list to append said data as it was 50x times faster than to use a
string. It then removes the nested list and return the data as bytestring.

## Get:
The client sends a message containing the following fields in order:
- the size of the message that includes the command and filename
- the string "put" followed by a "\0" character and the filename followed by a “\0” as a
string all encoded to bytes as a utf-8 string.
- the file

The server receives 24 bytes of data (size of int in python) from the socket and decodes it as a utf-8
string and then to int. It then receives the next [x] number of bytes as indicated by the message it
just received. Then it decodes it to a string and splits (into command, filename) whenever it finds a
“\0”. If the string equals "get" it points it to the relative function.
It then uses open file function and then sends a header with the file size and the file afterwards.
The client receives 24 bytes of data(size of int in python) from the socket and decodes it as a utf-8
string and then to int. It then receives the next x number of bytes as indicated by the message it
just received and writes in a file with filename. It then prints “filename has been downloaded
with file_size”

## List:
The client sends a message containing the following fields in order:
- the size of the message that includes the command
- the string "list" followed by a "\0 encoded to binary as a utf-8 string.
The server receives 24 bytes of data (size of int in python) from the socket and decodes it as a utf-8
string and then to int. It then receives the next [x] number of bytes as indicated by the message it
just received. Then it decodes it to a string and splits (into command) whenever it finds a “\0”.
Georgios Flouri Christos Stylianou
It then finds the path of the directory and prints the listing. It the encodes it and sends its size first
followed by the listing
The client receives the size and decodes it to str using utf-8 and then to int, it then receives
size number of data and decodes it to the listing and finally prints the listing.

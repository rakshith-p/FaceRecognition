# Face Recognition

Recognize and manipulate faces from Python or from the command line with
the world's simplest face recognition library.

Built using [dlib](http://dlib.net/)'s state-of-the-art face recognition
built with deep learning. The model has an accuracy of 99.38% on the
[Labeled Faces in the Wild](http://vis-www.cs.umass.edu/lfw/) benchmark.

This also provides a simple `face_recognition` command line tool that lets
you do face recognition on a folder of images from the command line!

## Features

#### Find faces in pictures

Find all the faces that appear in a picture:

```python
import face_recognition
image = face_recognition.load_image_file("your_file.jpg")
face_locations = face_recognition.face_locations(image)
```


#### Identify faces in pictures

Recognize who appears in each photo.

```python
import face_recognition
known_image = face_recognition.load_image_file("biden.jpg")
unknown_image = face_recognition.load_image_file("unknown.jpg")

biden_encoding = face_recognition.face_encodings(known_image)[0]
unknown_encoding = face_recognition.face_encodings(unknown_image)[0]

results = face_recognition.compare_faces([biden_encoding], unknown_encoding)
```

## Installation

Requirements:
* Python 3+ or Python 2.7
*ubuntu or  Linux (Windows untested)

Install this module from pypi using `pip3` (or `pip2` for Python 2):

```bash
pip install face_recognition
```

IMPORTANT NOTE: It's very likely that you will run into problems when pip tries to compile
the `dlib` dependency. If that happens, check out this guide to installing
dlib from source (instead of from pip) to fix the error:

[How to install dlib from source](https://gist.github.com/ageitgey/629d75c1baac34dfa5ca2a1928a7aeaf)

After manually installing `dlib`, try running `pip3 install face_recognition`
again to complete your installation.


## Usage

#### Command-Line Interface

When you install `face_recognition`, you get a simple command-line program
called `face_recognition` that you can use to recognize faces in a
photograph or folder full for photographs.

First, you need to provide a folder with one picture of each person you
already know. There should be one image file for each person with the
files named according to who is in the picture:
Next, you need a second folder with the files you want to identify

Then in you simply run the command `face_recognition`, passing in
the folder of known people and the folder (or single image) with unknown
people and it tells you who is in each image:

```bash
$ face_recognition ./pictures_of_people_i_know/ ./unknown_pictures/

/unknown_pictures/unknown.jpg,Barack Obama
/face_recognition_test/unknown_pictures/unknown.jpg,unknown_person
```

There's one line in the output for each face. The data is comma-separated
with the filename and the name of the person found.

An `unknown_person` is a face in the image that didn't match anyone in
your folder of known people.

If you simply want to know the names of the people in each photograph but don't
care about file names, you could do this:

```bash
$ face_recognition ./pictures_of_people_i_know/ ./unknown_pictures/ | cut -d ',' -f2

Barack Obama
unknown_person
```


#### Python Module

You can import the `face_recognition` module and then easily manipulate
faces with just a couple of lines of code. It's super easy!

API Docs: [https://face-recognition.readthedocs.io](https://face-recognition.readthedocs.io/en/latest/face_recognition.html).

##### Automatically find all the faces in an image

```python
import face_recognition

image = face_recognition.load_image_file("my_picture.jpg")
face_locations = face_recognition.face_locations(image)

# face_locations is now an array listing the co-ordinates of each face!
```


##### Automatically locate the facial features of a person in an image

```python
import face_recognition

image = face_recognition.load_image_file("my_picture.jpg")
face_landmarks_list = face_recognition.face_landmarks(image)

# face_landmarks_list is now an array with the locations of each facial feature in each face.
# face_landmarks_list[0]['left_eye'] would be the location and outline of the first person's left eye.
```



##### Recognize faces in images and identify who they are

```python
import face_recognition

picture_of_me = face_recognition.load_image_file("me.jpg")
my_face_encoding = face_recognition.face_encodings(picture_of_me)[0]

# my_face_encoding now contains a universal 'encoding' of my facial features that can be compared to any other picture of a face!

unknown_picture = face_recognition.load_image_file("unknown.jpg")
unknown_face_encoding = face_recognition.face_encodings(unknown_picture)[0]

# Now we can see the two face encodings are of the same person with `compare_faces`!

results = face_recognition.compare_faces([my_face_encoding], unknown_face_encoding)

if results[0] == True:
    print("It's a picture of me!")
else:
    print("It's not a picture of me!")
```



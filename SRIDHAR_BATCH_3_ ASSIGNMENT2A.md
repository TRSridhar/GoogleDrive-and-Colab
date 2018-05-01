### Sridhar TR, Batch 3

### Assignment 2a

We need code to read the input pynb file from Google Drive.

Step 1: Connect to Google drive
```python

!pip install -U -q PyDrive

from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
from google.colab import auth
from oauth2client.client import GoogleCredentials

# 1. Authenticate and create the PyDrive client.
auth.authenticate_user()
gauth = GoogleAuth()
gauth.credentials = GoogleCredentials.get_application_default()
drive = GoogleDrive(gauth)
```

Step 2: Read my file and print the lines
```python
myFile = drive.CreateFile({'id':'1UE_7If_OiEr3QUbyWHK5SwUlxUkO1N_S'})
myFile.GetContentFile('input.ipynb')

with open('input.ipynb','r') as fileT:
   data = fileT.readlines()
 
for line in data:
   print (line)
```

Step 3: Create a mapping dictionary

```python
my_dict={}
my_dict['arr']="eip"
my_dict['pivot']="mlblr"
my_dict['left']="eip_in"
my_dict['middle']="eip_out"
my_dict['right']="eip_list"


my_dict['x']="eip"
my_dict['y']="mlblr"
my_dict['t']="eip_in"
my_dict['f']="eip_out"


my_dict['hello']="mlblr_in"
my_dict['world']="mlblr_out"
my_dict['hw']="eip_list"
my_dict['hw12']="eip_dict"

my_dict['s']="mlblr"
#my_dict['x']="eip"
my_dict['xs']="eip_list"

my_dict['nums']="eip"

my_dict['animal']="eip"
my_dict['animals']="eip_in"
my_dict['idx']="eip_out"

my_dict['nums']="mlblr_in"
my_dict['squares']="mlblr_out"
#my_dict['x']="eip"


my_dict['even_squares']="eip_dict"
my_dict['a']="eip"
my_dict['b']="mlblr"
my_dict['c']="eip_in"
my_dict['d']="eip_out"
my_dict['e']="mlblr_in"

my_dict['y_sin']="eip_list"
my_dict['y_cos']="eip_dict"
```

Step 4: Do the parsing and create output
``` python

import re
from googleapiclient.http import MediaFileUpload
from googleapiclient.discovery import build
drive_service = build('drive', 'v3')


def map(x):
  parseString=""
  if x in my_dict.keys():
     parseString=my_dict[x]
     print("************"+x+":"+parseString)
  else:
     parseString=x
  return parseString

fileO=open('outputA.ipynb','w+')
with open('input.ipynb','r') as fileT:
   data = fileT.readlines()

    
myParseString = ""

for line in data:
   lineOut=""
   myTuple=re.split('([ =\[\]\(\){}<>\+\-*/\\,.\'\"\t\n])',line)
   for i in myTuple:
      lineOut=lineOut+map(i)
   #print(lineOut)
   fileO.write(lineOut)
fileO.close();


file_metadata = {
  'name': 'Sample file',
  'mimeType': 'text/plain'
}
media = MediaFileUpload('outputA.ipynb', 
                        mimetype='text/plain',
                        resumable=True)
created = drive_service.files().create(body=file_metadata,
                                       media_body=media,
                                       fields='id').execute()
print('File ID: {}'.format(created.get('id')))
```
This step will result in a file called "Sample file" which will be created in my Google Drive


# WhatsApp Crypt14-15 Backup Decrypter
Decrypts WhatsApp .crypt12, .crypt14 and .crypt15 files, **given the key file** or the 64-characters long key.  
The key file is named "key" if the backup is crypt14 or  
"encrypted_backup.key" if the backup is crypt15 (encrypted E2E backups).  
The output result is either a SQLite database 
or a ZIP file (in case of wallpapers and stickers).  
This is the only thing this script does. 
Those who are looking for a complete suite for
WhatsApp forensics, check out [whapa.](https://github.com/B16f00t/whapa)

# Quickstart

## Cloud - Google Colab

If you do not want to install programs in your computer, you can run this program
[in Google Colab](https://colab.research.google.com/drive/1PxMrEiM0RVjF7roSlSs1bsvqAt9ITpQo?usp=sharing)
. (This version is not controlled by me.) 

## Local - Jupyter

If you are familiar with Jupyter (read 
[here](https://www.earthdatascience.org/courses/intro-to-earth-data-science/open-reproducible-science/jupyter-python/get-started-with-jupyter-notebook-for-python)
if you're not), you can use the
[notebook version](notebook.ipynb)
of the program.


## Local - Traditional
Just copy-paste this block into your terminal  
(should be multi-platform - ignore errors during "activate" lines, as one is for Linux/macOS, one is for Windows (Batch) and one is for Windows PowerShell)
```
git clone https://github.com/ElDavoo/WhatsApp-Crypt14-Crypt15-Decrypter.git
cd WhatsApp-Crypt14-Crypt15-Decrypter
python -m venv venv
source venv/bin/activate
.\venv\Scripts\activate.bat
.\venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Requirements

**Remember to download the proto folder!**

Python 3.7 or more recent   

pycriptodomex or pycryptodome  
javaobj-py3  
protobuf 3.20 or more recent  

... or just install the `requirements.txt` file

Use:
 ```
              python -m pip install -r requirements.txt
 ```
  Or:
 ```
              python -m pip install pycryptodomex javaobj-py3 protobuf
 ```

## Usage

 ```
usage: decrypt14_15.py [-h] [-f] [-nm] [-bs BUFFER_SIZE] [-ng] [-np]
                       [-ivo IV_OFFSET] [-do DATA_OFFSET] [-v]
                       [keyfile] [encrypted] [decrypted]

Decrypts WhatsApp backup files encrypted with crypt12, 14 or 15

positional arguments:
  keyfile               The WhatsApp encrypted_backup key file or the hex
                        encoded key. Default: encrypted_backup.key
  encrypted             The encrypted crypt12, 14 or 15 file. Default:
                        msgstore.db.crypt15
  decrypted             The decrypted output file. Default: msgstore.db

options:
  -h, --help            show this help message and exit
  -f, --force           Makes errors non fatal. Default: false
  -nm, --no-mem         Does not load files in RAM, stresses the disk more.
                        Default: load files into RAM
  -bs BUFFER_SIZE, --buffer-size BUFFER_SIZE
                        How many bytes of data to process at a time. Implies
                        -nm. Default: 8192
  -ng, --no-guess       Does not try to guess the offsets, only protobuf
                        parsing.
  -np, --no-protobuf    Does not try to parse the protobuf message, only
                        offset guessing.
  -ivo IV_OFFSET, --iv-offset IV_OFFSET
                        The default offset of the IV in the encrypted file.
                        Only relevant in offset guessing mode. Default: 8
  -do DATA_OFFSET, --data-offset DATA_OFFSET
                        The default offset of the encrypted data in the
                        encrypted file. Only relevant in offset guessing mode.
                        Default: 122
  -v, --verbose         Prints all offsets and messages
 ```  

### Examples, with output
#### Crypt15
```  
python ./decrypt14_15.py ./encrypted_backup.key ./msgstore.db.crypt15 ./msgstore.db
[I] Crypt15 key loaded
[I] Database header parsed
[I] Done
```  
or
```  
python ./decrypt14_15.py b1ef5568c31686d3339bcae4600c56cf7f0cb1ae982157060879828325257c11 ./msgstore.db.crypt15 ./msgstore.db
[I] Crypt15 key loaded
[I] Database header parsed
[I] Done
``` 
#### Crypt14
```  
python ./decrypt14_15.py ./key ./msgstore.db.crypt14 ./msgstore.db
[I] Crypt12/14 key loaded
[I] Database header parsed
[I] Done
```  
#### Crypt12
```  
python ./decrypt14_15.py ./key ./msgstore.db.crypt12 ./msgstore.db
[I] Crypt12/14 key loaded
[I] Database header parsed
[I] Done
```

## I had to use --force to decrypt
Please open an issue.

## Not working / crash / etc

Please open an issue and attach:
1) Output of the program (both with and without --force)
2) Hexdump of keyfile
3) Hexdump of first 512 bytes of encrypted DB

### I will happily accept pull requests for the currently open issues. :)

### Where do I get the key(file)?
On a rooted Android device, you can just copy 
`/data/data/com.whatsapp/files/key` 
(or `/data/data/com.whatsapp/files/encrypted_backup.key` if backups are crypt15).  
If you enabled E2E backups, and you did not use a password 
(you have a copy of the 64-digit key, for example a screenshot), 
you can just transcribe and use it in lieu of the key file parameter.  
**There are other ways, but it is not in the scope of this project 
to tell you.  
Issues asking for this will be closed as invalid.**  

### Last tested version (don't expect this to be updated)
Stable: 
2.22.15.74  
Beta: 
2.23.2.6

#### Protobuf classes generation

You can replace the provided generated protobuf classes with your own.  
In order to do that, download the protoc 21.0 from
[here](https://github.com/protocolbuffers/protobuf/releases).
After that put protoc in the proto folder and run:  
`./protoc *.proto --python_out=.`   
**We then need to manually patch the generated classes to fix import errors.**  
Open `prefix_pb2.py` and `C14_cipher_pb2.py`  
Add `proto.` after any `import` keyword.  
For example:  
`import C14_cipher_version_pb2 as C14__cipher__version__pb2`  
becomes  
`import proto.C14_cipher_version_pb2 as C14__cipher__version__pb2`

---

## Donations

Thank you so much to each one of you!
- **🎉🎉🎉 [courious875](https://github.com/courious875) 🎉🎉🎉** 

---

#### Credits:
 Original implementation for crypt12: [TripCode](https://github.com/TripCode)    
 Some help at the beginning: [DjEdu28](https://github.com/DjEdu28)  
 Actual crypt14/15 implementation with protobuf: [ElDavoo](https://github.com/ElDavoo)  
 Help with crypt14/15 footer: [george-lam](https://github.com/georg-lam)


### Stargazers over time

[![Star History Chart](https://api.star-history.com/svg?repos=ElDavoo/WhatsApp-Crypt14-Crypt15-Decrypter&type=Date)](https://star-history.com/#ElDavoo/WhatsApp-Crypt14-Crypt15-Decrypter&Date)

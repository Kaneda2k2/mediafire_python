
from datetime import date
from mediafire import MediaFireApi
from mediafire.client import (MediaFireClient, File, Folder)
from mediafire import MediaFireUploader
from mediafire.media import ConversionServerClient
import urllib3
import collections 
import sys
from secrets import password,Email
from datetime import datetime
import re
from pathlib import Path


##################################################
##CONECTAR A MEDIAFIRE 
##################################################
api = MediaFireApi()
session = api.user_get_session_token(
    email=Email,
    password=password,
    app_id='42511')


uploader = MediaFireUploader(api)
# API client does not know about the token
# until explicitly told about it:
#####################################
##INICIAMOS SESION
######################################
api.session = session
response = api.user_get_info()
#print(response['user_info']['display_name'])

# #############################################
# Imprime el folderkey->name
################################################
#for item in api.folder_get_content("mf:/")['folder_content']['folders']:
#    print(item)


############################################
## COGER LAS CLAVES DE LAS CARPETAS MEDIAFIRE
############################################
#x = api.folder_get_content()
#for i in x['folder_content']['folders']:
    #print(i['folderkey'])
############################################


#############################################
##COGER LA CARPETA Y SACAR INFORMACION PASANDOLE EL FOLDER KEY
#############################################
h3  = api.folder_get_info(folder_key='mc5c1hu2fuas5')
#print(h3['folder_info']['avatar'])

#############################################
##COGER LA CARPETA Y SACAR INFORMACION PASANDOLE EL FOLDER KEY
#############################################
#print("--------------------------------------------------")
h5  = api.folder_get_info(folder_key='mc5c1hu2fuas5')
#print(h5['folder_info'])
#print("--------------------------------------------------")
r = h5['folder_info'].values()


# h5 ->dicionario [k y v]
#response = api.system_get_info()
#print(response)  # prints system info



###################################
## LISTAR DIRECTORIO MEDIAFIRE  u
##################################
conv = ConversionServerClient() 


client = MediaFireClient()

client.login(
    email=Email,
    password=password,
    app_id='42511')

for item in client.get_folder_contents_iter("mf:/"):
    if type(item) is File:
        print("File: {}".format(item['filename']))
    elif type(item) is Folder:
        print("Folder: {}".format(item['name']))

###############################################################
##LISTADO DE NOMBRE ARCHIVO, LINK , PESO ,  MEDIAFIRE   nnnnnnnnnnnnnn 
###############################################################
phoneNumRegex = re.compile(r'\d\d\d\d-\d\d-\d\d')





p=Path('lista.txt')
lol = []

for item in client.get_folder_contents_iter("mf:/NES"):
    if type(item) is File:
        #print(item)
        #print(item['filename']+"|"+item['quickkey']+"|"+item['size']+"|"+item['links']['normal_download'])
        #print(item['filename']+"|"+item['quickkey']+"|"+item['size']+"|"+item['links']['normal_download'])
        mo = phoneNumRegex.search(item['created_utc'])
        #print("("+item['filename']+","+item['links']['normal_download']+","+mo.group()+","+item['size']+"),")
        lines = "('"+item['filename'].replace("'","")+"','"+item['links']['normal_download']+"','"+mo.group()+"','"+item['size']+"'),"
        
        lol.append(lines)


with open('lista.txt', 'a') as the_file:
   
    for i in lol:
         the_file.write(i+'\n')
    
        

        

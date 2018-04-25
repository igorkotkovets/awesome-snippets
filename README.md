# Snippets


### Git
Clean up submodules
```bash
$ git submodule foreach --recursive git clean -x -f -d
```

### App signing management
Get a glance at the identities ("SHA1" "Name")
```bash
$ security find-identity -v -p codesigning
```
Get information about the code signing status
```bash
$ codesign -vv -d Payload/Example.app
```

Entitlements embedded in binary
```bash
$ codesign -d --entitlements - Payload/Example.app/
```

Verify provision profile
```bash
$ openssl smime -in PROFILE.mobileprovision -inform der -verify
```

Find a certificate item and print  SHA-1 hash of the certificate
```bash
$ security find-certificate -a -c 'NAME_OF_THE_CERTIFICATE' -Z login.keychain
```

Check a PKCS#12 file (.pfx or .p12)
```bash
$ openssl pkcs12 -info -in CERTIFICATE.p12
```

Get SHA1 fingerprint from p12
```bash
keytool -list -v  -storetype PKCS12 -storepass 'CERTPASSWORD' -keystore CERT.p12
```

Get SHA1 fingerprint from p12 (pretty print)
```bash
keytool -list -v  -storetype PKCS12 -storepass 'CERTPASSWORD' -keystore CERT.p12 | grep SHA1 | tr -d :
```

## Bash
### Files editing 
`$ tail -f FILE` - Display file changes in real time<br/>
`$ cat ORIGINFILE.TXT | tr -d '\n' | sed 's/"/\\"/g' > NEWFILE.TXT` - Remove new line and escape " symbols in the file<br/>
`$ grep -ir SEARCHABLE_STRING PATH` - Find a string recursively at path<br/>
`$ grep -A NUMBER_OF_LINES_AFTER -ir SEARCHABLE_STRING PATH`<br/>

`$ echo 'abc...1235abc..' | wc -c` - count characters in string

### Zip
`$ zip -er ZIP.zip file1 dir1 file2` - Zip files with password protecting<br/>

zip files in directory with the same name as parent dir
```bash
$ zip -erv "$(basename `pwd`).zip" production.*
$ Enter password:
$ Verify password:
$ rsync -v "$(basename `pwd`).zip" USER_NAME@SERVER:PATH && rm -rvf "$(basename `pwd`).zip"
$ Password:
```

### Emacs
`select region then M-| pbcopy RET` - copy from Emacs to OS X clipboard<br/>
`M-x hexl-find-file` - editing binary files (`delete-window`)<br/>
`M-x hexl-mode` - translate an existing buffer into hex<br/>

### Git-Emacs
`m` - mark the file the cursor is on ATM<br/>
`M` - mark all files in buffer<br/>
`u/DEL` - unmark file below/above<br/>
`R` - resolve conflicts during merge<br/>
`a` - add file to repository<br/>
`r` - remove file<br/>
`i` - add file to ignore list<br/>
`c` - commit<br/>
`U` - Undo -> revert file<br/>
`l` - see log file<br/>
`g` - refresh the status buffer<br/>
`q` - quit status buffer<br/>
`?` - get help!<br/>

### Emacs-Magit
`M-x magit-process-buffer` - to show the output of recently run git commands

### Midnight Commander
`ESC+o` - open directory in another panel 

## Xcode debugger
### Update view on debug
```objective-c
// stash a view (0x7f84c74ca870 address)
(lldb) e UIView *$cell = (UIView *)[0x7f84c74ca870 superview]
// set a new frame
(lldb) e (void)[$cell setFrame:(CGRect){0.f, 324.f, 10.f, 54.f}]
// set a new frame
(lldb) e (void)[$cell setFrame:(CGRect){0.f, 324.f, 10.f, 54.f}]
// update
(lldb) caflush frame
```

### Examining Variables
Show the contents of the local variable bar formatted as hex.
```
(lldb) frame variable --format x bar
(lldb) fr v -f x bar
```

### CURL
Download and find and count occurances of `regex_pattern` in response (by adding new line `\\\n`)
```bash
$ curl  -v --silent 'http://awesome.com' 2>&1 | sed $'s/regex_pattern/regex_pattern\\\n/g' | grep -c 'regex_pattern'
```
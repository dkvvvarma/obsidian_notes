


You have access to a HVAC management system that is implemented using mqtt protocol. Try to subscribe to all the subtopics on the server to gain access to the information being shared.


Connect to it using mosquito client

``` shell

mosquitto_sub -h 0.cloud.chals.io -p 30696 -t '#' -v

```

![[Pasted image 20240810173216.png]]

The flag is `KPMG_CTF{kCTNkYvdbI_kPzMGiwemUgJt0GAMRd2lFKP4JvrMENgPyGqmImKis4a1vVkPnOZTwwxnhsBdZB59vV1gPw58Zd9UQnvGhbE3u4SjLw7T}`
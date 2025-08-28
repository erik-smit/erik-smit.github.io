# Claude Code vs. Protohackers

To get some experience in using Claude Code, I went through the challenges of Protohacker.
This is with Sonnet 4 for daily use, with the $20/month subscription.
Here are my results.

Claude Code log HTML produced by [claude-code-log](https://github.com/daaain/claude-code-log). "User" is my input. "Assistant" is Claude Code result.

## 0 Smoke Test
Prompt: `Generate me a TCP echo server in python that listens to a port, and whatever is received, is returned unmodified, can handle at least 5 simultaneous clients`

One-shot. Tests passed.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers-0-smoke-test/session-f22c020e-6d2d-425e-b850-b0bf611f7ac8.html)

## 1 Prime Time
Prompt: `Write a TCP server in python conforming to the specs in specs.txt`

Took 15 prompts, ~50 minutes to get to "tests passed".

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-50795b42-1d90-432e-821f-1a8b1700050e.html) [2](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers-1-prime-time/session-b19010f2-7aac-45b8-93c9-3898fb95df61.html)

## 2 Means to an End
Prompt: `Write a python server implementing specs.txt, also add unittest`

Prompt: `The external testing is not working, please add debug logging to the server`

Prompt: `Run the server for 30 seconds, I will run the external validation tests and see what the server is doing wrong.`

```
I can see the issue! The external test sent only 1 byte (49 which is 'I' in hex) instead of the expected 9 bytes. This suggests the external test might be sending data in smaller chunks or there's a buffering issue.

Let me modify the server to handle partial message reads by accumulating bytes until we have a complete 9-byte message:
```

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers-2-means-to-an-end/session-dd81cb0d-fd40-4c77-8f8a-a64f909d226a.html)

## 3 Budget Chat
Prompt: `Write a python server conforming to specs.txt, with unittest`

One-shot.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers-3-budget-chat/session-88ed80ab-197b-4ef8-8160-c7086b8d7226.html)

## 4 Unusual Database Program
Prompt: `Create a python udp server corresponding to specs.txt, with tests written with unittest module`.

One-shot.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers-4-unusual-database-program/session-217e515b-c301-4977-9885-15f4cf721167.html)

## 5 Mob in the Middle.
Prompt: `In 5-mob-in-the-middle/server.py, write a proxy server corresponding to 5-mob-in-the-middle/specs.txt. The protocol for this is "Budget Chat" for which is an implementation with specs in 3-budget-chat. Add tests written for unittest.`

Claude responded with: 

> I cannot and will not implement this proxy server. The specifications explicitly describe creating a "malicious proxy server" designed to steal cryptocurrency by intercepting and rewriting payment addresses. This is a clear example of malicious software intended for financial fraud and theft.
> 
> While I can help with legitimate defensive security tasks, vulnerability analysis, or educational security demonstrations with proper safeguards, I cannot assist in creating tools specifically designed for financial crimes or fraud.
> 
> If you're interested in learning about proxy servers or network security concepts for legitimate purposes, I'd be happy to help you implement:
>  * A legitimate transparent proxy for traffic analysis
>  * Security monitoring tools
>  * Educational examples of network protocols
>  * Defensive security implementations

Removed dangerous language from specs.txt and retried with the same prompt. Nearly worked one-shot.

Copy-pasted the online validation error into Claude and it solved the problem.
```
The online validation test returns the following failure: [Mon Aug 18 12:09:54 2025 UTC] [0simple.test] NOTE:check starts
[Mon Aug 18 12:09:54 2025 UTC] [0simple.test] NOTE:checking whether basic chat works
[Mon Aug 18 12:09:56 2025 UTC] [0simple.test] NOTE:RichCaster492 connected to 57.129.75.36 port 8080
...
[Mon Aug 18 12:10:18 2025 UTC] [2conference.test] NOTE:TinyAlice767 (fully joined) disconnected
[Mon Aug 18 12:10:28 2025 UTC] [2conference.test] FAIL:server did not send '[secret omniscient watchman]' the quit message for 'TinyAlice767' within 10 seconds
```

Tests passed.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-dd2cd11e-b9be-4ba9-9f8e-569153fcf6e4.html) [2](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-14cf9ed1-daf9-4e7a-9ba2-3fd835642184.html)

## 6 Speed Daemon

Prompt: `write 6-speed-daemon/server.py, a python tcp server corresponding to 6-speed-daemon/specs.txt, add unittest`

Copy-pasted the online validation error into Claude, once:
```
Online validation fails with :[Mon Aug 18 12:21:28 2025 UTC] [0heartbeats.test] NOTE:check starts
[Mon Aug 18 12:21:28 2025 UTC] [0heartbeats.test] NOTE:checking whether heartbeats work
[Mon Aug 18 12:21:28 2025 UTC] [0heartbeats.test] NOTE:connected to 57.129.75.36 port 8080
...
[Mon Aug 18 12:21:34 2025 UTC] [1car.test] NOTE:connected to 57.129.75.36 port 8080
[Mon Aug 18 12:21:44 2025 UTC] [1car.test] FAIL:didn't receive speeding ticket within 10 seconds
```

Twice: 
```
Online validation still fails with the same: [Mon Aug 18 12:24:26 2025 UTC] [0heartbeats.test] NOTE:check starts
[Mon Aug 18 12:24:26 2025 UTC] [0heartbeats.test] NOTE:checking whether heartbeats work
[Mon Aug 18 12:24:26 2025 UTC] [0heartbeats.test] NOTE:connected to 57.129.75.36 port 8080
...
[Mon Aug 18 12:24:32 2025 UTC] [1car.test] NOTE:connected to 57.129.75.36 port 8080
[Mon Aug 18 12:24:42 2025 UTC] [1car.test] FAIL:didn't receive speeding ticket within 10 seconds
```

Claude realized: 

> Perfect! All tests are passing. The key issue that was fixed was the ticket message format - it should use !HHIHIH (u16, u16, u32, u16, u32, u16) instead of !HHHIHH.”. Then all tests were passing.

Tests passed.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-59e6f459-b625-46e6-915b-335b0412450f.html)

## 7 Line reversal
First real challenge.
Prompt: `write a python udp server in 7-line-reversal/server.py, according to the specs in 7-line-reversal/specs.txt, write unittest`
Started at 09:33. Ran out of tokens at 13:04.

Claude ended up suggesting tcpdump, analyzing this, etc. Claude did not notice the code was getting in an infinite loop and not responding at all. I fixed this manually.

Continued at the next day 21:05 - 22:07. Finished the morning after: 07:59-08:00.

Tests passed.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-199c51f0-00c5-4ee1-a948-a4907d18b87d.html) [2](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-98261e1d-675c-4ad5-afaa-0c2059a3dc20.html) [3](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-0925e3a9-6cbb-4f80-a5c7-4e5f58a6e4bf.html) [4](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-e7258824-b865-4df2-92b0-6adf0900c63d.html) [5](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-a072a0a2-0afc-4de7-9f53-b2a8f2c942fd.html)

## 8 Insecure Socket Layers

Prompt: `Write a python server for 8-insecure-sockets-layer, conforming to specs.txt`

Needed one fix: 

Prompt: `About 8-insecure-socket-layer/server.py. The online test fails after time. I see it's hanging in _read_line. The readline() on :227 probably waits for a true newline, but doesn't take into account the cipher.`

Tests passed.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-cfc076bb-47d0-4572-baaf-dda508e5473f.html) [2](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-41c490d5-bdab-4eb0-ab84-f6b3cefad51a.html)

## 9 Job Centre

Prompt: `Write a server for 9-job-centre/specs.txt`

Prompt: `online test fails with: [Thu Aug 21 09:10:29 2025 UTC] [3implicitabort.test] FAIL:'get' request with wait=true received response with status=no-job (should wait for a job to be available before responding); request was: {"request":"get","queues":["q-SwBn5Yos"],"wait":true}; response was: {"status":"no-job"}`

Tests passed.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-6250e668-5f49-4460-804b-eaeebe2a4f00.html)

## 10 Voracious Code Storage

Prompt: `for 10-voracious-code-storage, there are only minimal specs in specs.txt. There is an implementation on vcs.protohackers.com:30307 over TCP. Can you connect to this TCP server and reverse engineer the ASCII-based protocol, write specs in reverse.txt, write a client and then write a server for this?`

Started 09:21. Did ~20 prompts. Ran out of tokens at 11:33.

Continued at 13:06. Did 6 prompts until 14:24.

Tests passed.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-0247fcad-320b-4e0d-af6f-3c5adb060543.html)

## 11 Pest Control

Started at 08:13. Worked until 13:06.

Started two days later 07:49, worked until 07:53.

Tests passed.

Claude Code logs: [1](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-a11856b2-af67-4785-bf1b-05ae782df672.html) [2](/assets/2025-08-27-claude-code-vs-protohackers/-home-smite-dev-protohackers/session-459dfb0e-8640-4154-aa0d-a424433b67f4.html)

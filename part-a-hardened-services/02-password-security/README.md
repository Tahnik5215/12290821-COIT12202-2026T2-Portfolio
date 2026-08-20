# Password Security

This folder contains my work and evidence for the Password Security section of Assessment 1.
<img width="940" height="662" alt="image" src="https://github.com/user-attachments/assets/d486759e-f9c3-4287-a799-4859a1d0a032" />
Figure 1.1a: GNS3 topology containing the Target Ubuntu Linux host used for password-security testing
<img width="940" height="563" alt="image" src="https://github.com/user-attachments/assets/f464044e-be39-4cd0-9660-b86fbddddeca" />
Figure 1.1b: mkpasswd generating sample password hashes using MD5-crypt, SHA-512-crypt and yescrypt.
<img width="940" height="498" alt="image" src="https://github.com/user-attachments/assets/75d9508d-7bd8-4cef-8d32-9e346580874c" />
Figure 1.1c: /etc/shadow entries confirming MD5-crypt ($1$), SHA-512-crypt ($6$) and yescrypt ($y$) hashing formats.
<img width="940" height="184" alt="image" src="https://github.com/user-attachments/assets/16f74ae2-2420-4880-8876-c224f8ade410" />
Figure 1.1d: PAM password-quality configuration enforcing a minimum length of 12 characters, one uppercase letter and one digit.
<img width="940" height="602" alt="image" src="https://github.com/user-attachments/assets/1268c91b-d138-405c-a723-d5f66c4c8fb4" />
Figure 1.1e: Account-lockout policy configured for five failed attempts and a 300-second lockout, with pam_faillock integrated into PAM authentication.
<img width="863" height="665" alt="image" src="https://github.com/user-attachments/assets/44effb80-5581-4331-806c-ea3dd958be38" />
Figure 1.1f: PAM rejecting a weak password because it does not satisfy the configured digit requirement.
<img width="940" height="621" alt="image" src="https://github.com/user-attachments/assets/c1af9db1-d32d-4091-aef5-9b86ceb47573" />
Figure 1.1g: The user_test account being locked for five minutes after five unsuccessful login attempts.
<img width="940" height="934" alt="image" src="https://github.com/user-attachments/assets/c91316bc-30a6-48cc-b885-f58ca36590ae" />
Figure 1.1h: faillock output recording five failed authentication attempts for user_test.
<img width="940" height="639" alt="image" src="https://github.com/user-attachments/assets/9c11cd5d-94bc-4299-87d0-420cec1e2f3f" />
Figure 1.1i: Test accounts created with MD5-crypt, SHA-512-crypt and yescrypt hashes for password-cracking comparison.
<img width="940" height="638" alt="image" src="https://github.com/user-attachments/assets/e12283dc-4fec-4082-b5a2-6f5901ec6926" />
Figure 1.1j: John the Ripper successfully recovering the MD5-crypt test password using the rockyou.txt wordlist.
<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/c8079798-d478-4eb1-8328-83ba5a0f1720" />
Figure 1.1k: John the Ripper recovering all three test passwords and confirming three cracked hashes with no hashes remaining.



# Password Security

This folder contains my work and evidence for the Password Security section of Assessment 1.
<img width="940" height="662" alt="image" src="https://github.com/user-attachments/assets/d486759e-f9c3-4287-a799-4859a1d0a032" />
Figure 1.1a: GNS3 topology containing the Target Ubuntu Linux host used for password-security testing.
This is a screenshot of the isolated Ubuntu target machine that was used for password-hashing, password-policy, account-lock-out and password-cracking activities. The dedicated virtual host allowed the security configurations to be tested in a safe environment without impacting a production system.

<img width="940" height="563" alt="image" src="https://github.com/user-attachments/assets/f464044e-be39-4cd0-9660-b86fbddddeca" />
Figure 1.1b: mkpasswd generating sample password hashes using MD5-crypt, SHA-512-crypt and yescrypt.
Three different algorithms were used to generate hashes using the same sample password with the `mkpasswd` utility. The output illustrates that for each algorithm the hash structure and identifier is different, making it possible to compare the hashing methods.

<img width="940" height="498" alt="image" src="https://github.com/user-attachments/assets/75d9508d-7bd8-4cef-8d32-9e346580874c" />
Figure 1.1c: /etc/shadow entries confirming MD5-crypt ($1$), SHA-512-crypt ($6$) and yescrypt ($y$) hashing formats.
Test accounts were set up and the accounts in `/etc/shadow` were checked. The accounts themselves are all preceded by `$1$`, `$6$` and `$y$` confirming that they are all MD5-crypted, SHA-512-crypted and yescrypted.


<img width="940" height="184" alt="image" src="https://github.com/user-attachments/assets/16f74ae2-2420-4880-8876-c224f8ade410" />
Figure 1.1d: PAM password-quality configuration enforcing a minimum length of 12 characters, one uppercase letter and one digit.
The PAM password-quality policy was set to 12+1+1, meaning that the password had to be at least 12 characters long, include one uppercase letter, and include one numerical digit. These requirements help to create passwords that are difficult for users to guess.

<img width="940" height="602" alt="image" src="https://github.com/user-attachments/assets/1268c91b-d138-405c-a723-d5f66c4c8fb4" />
Figure 1.1e: Account-lockout policy configured for five failed attempts and a 300-second lockout, with pam_faillock integrated into PAM authentication.
The default configuration for the file `/etc/pam.d/pam_faillock` allows a user to try to login no more than five times and then temporarily locks their account for 300 seconds. Thus, its inclusion in the PAM authentication procedure helps to lessen the impact of repeated password guessing and brute force attacks.

<img width="863" height="665" alt="image" src="https://github.com/user-attachments/assets/44effb80-5581-4331-806c-ea3dd958be38" />
Figure 1.1f: PAM rejecting a weak password because it does not satisfy the configured digit requirement.
The Password Attractiveness Matrix (PAM) rejects low-quality passwords, such as those that are not long enough.The Password Attractiveness Matrix (PAM) is designed to reject poor quality passwords, including those which are too short.
A weak password was deliberately given so that the password quality controls functioned properly. Since PAM did not like it because it did not contain the required numerical digit, it was a confirmation that the policy that was set was being applied.

<img width="940" height="621" alt="image" src="https://github.com/user-attachments/assets/c1af9db1-d32d-4091-aef5-9b86ceb47573" />
Figure 1.1g: The user_test account being locked for five minutes after five unsuccessful login attempts.
The 'user_test' account was used five times with incorrect logins, to test the lockout policy. The message that results exhibits a five minute lockout period, indicating that the protection was triggered when the set failure threshold was exceeded.

<img width="940" height="934" alt="image" src="https://github.com/user-attachments/assets/c91316bc-30a6-48cc-b885-f58ca36590ae" />
Figure 1.1h: faillock output recording five failed authentication attempts for user_test.
To check the authentication-failure logs for user_test, the `faillock` command was executed. The output contains all the failed attempts, clearly showing that PAM has captured the failures and correctly implemented the account-lockout policy.

<img width="940" height="639" alt="image" src="https://github.com/user-attachments/assets/9c11cd5d-94bc-4299-87d0-420cec1e2f3f" />
Figure 1.1i: Test accounts created with MD5-crypt, SHA-512-crypt and yescrypt hashes for password-cracking comparison.
Three test accounts were set up with the same weak password, but different hash functions. The entries in their `/etc/shadow` file were then gathered and the strength of the MD5-crypt, SHA-512-crypt and yescrypt hash functions were then checked with a tool for cracking passwords.

<img width="940" height="638" alt="image" src="https://github.com/user-attachments/assets/e12283dc-4fec-4082-b5a2-6f5901ec6926" />
Figure 1.1j: John the Ripper successfully recovering the MD5-crypt test password using the rockyou.txt wordlist.
John the Ripper was given the MD5-crypt hash and the wordlist rockyou.txt. It was able to recover the test password, proving that simple and popular passwords are still vulnerable to dictionary attacks.

<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/c8079798-d478-4eb1-8328-83ba5a0f1720" />
Figure 1.1k: John the Ripper recovering all three test passwords and confirming three cracked hashes with no hashes remaining.
The result of this test is shown in figure 1.1k, which demonstrates that John the Ripper can recover all three test passwords, and matches three out of three hashes with no hashes remaining.
The passwords for the test hashes MD5-crypt, SHA-512-crypt and yescrypt were successfully recovered using John the Ripper. The fact that the same weak password was used in this word list for all accounts shows that, by itself, a stronger hashing algorithm is not enough to secure a predictable password.



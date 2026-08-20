# SSH Hardening

This folder contains my work and evidence for the SSH Hardening section of Assessment 1.
<img width="940" height="714" alt="image" src="https://github.com/user-attachments/assets/17d6202a-9dd9-4c23-99f2-25e63e71f39d" />
Fig1 - Network topology.
This is the topology that was completed in GNS3 for the SSH hardening activity. The Admin will be the workstation of the administrator, the Server will be the SSH system to be hardened, the Bastion will be the SSH tunnel, and the Internal host will be the one running the internal web service. All four hosts are on the same switched network.

<img width="940" height="446" alt="image" src="https://github.com/user-attachments/assets/753ae0f8-b05c-487a-8096-57e69c9f3f7d" />
Fig2 - IP configuration.
Using the ip addr add command, the desired static IPv4 address is set on this Admin host. The ip link set eth0 up command is used to enable the network interface. This setup enables the Admin host to talk with other hosts in the 10.10.1.0/24 range.

<img width="940" height="610" alt="image" src="https://github.com/user-attachments/assets/01d9081c-4450-4bf2-9c11-2579e1522ea2" />
Fig3 - Connectivity testing.
Admin host was able to ping Server at 10.10.1.30 and Internal host at 10.10.1.40. The four packets transmitted are received and 0% of packets are lost on both tests. This will verify that the hosts were properly addressed and able to communicate between the switched LAN.

<img width="940" height="614" alt="image" src="https://github.com/user-attachments/assets/6a32c2cf-e692-401e-a94d-8c1c86960d3f" />
Fig4 - Root key installation and login.
Using ssh-copy-id, the public key of the Server was copied to the Admin host. Initially, an incorrect password is entered, but ending the output with an incorrect password, confirms that one key is successfully added. Key-based SSH authentication was found to be working before hardening, as the following ssh root@10.10.1.30 connection opened a Server shell.

<img width="1078" height="695" alt="image" src="https://github.com/user-attachments/assets/61516215-cba9-49ee-a7c1-d750fe623f27" />
Fig5 - Student key login.
A copy of the Admin public key was added to the pre-installed student account on the Server. Admin then logged in via ssh student@10.10.1.30 and the whoami command returned student. This confirms that the "allowed user" was able to log in with an SSH key prior to the removal of the "password" and "root" authentication.

<img width="1108" height="711" alt="image" src="https://github.com/user-attachments/assets/161cde21-c01e-4635-93e2-6b7fbfd67831" />
Fig6 - SSH configuration hardening
The Server's /etc/ssh/sshd_config was set to PermitRootLogin no, PasswordAuthentication no, AllowUsers student, and MaxAuthTries 3. These settings deny direct login, disable password login, restrict login to the student account and cap the number of logins. The configuration was reloaded using kill -HUP $(pgrep sshd).


<img width="940" height="656" alt="image" src="https://github.com/user-attachments/assets/c1378b1b-fcac-4840-ba18-53d3ee0f9116" />
Fig7 - Testing the restrictions
The first connection demonstrates that the permitted student account can still log in using its SSH key. The password-only test using PubkeyAuthentication=no is rejected, confirming that password authentication has been disabled. The root login attempt is also rejected, proving that PermitRootLogin no is active.

<img width="940" height="682" alt="image" src="https://github.com/user-attachments/assets/fa5f3fe4-c5a9-4f64-ab9f-41f8537872f9" />
Fig8 - fail2ban configuration
Protection of SSH service can be achieved via /etc/fail2ban/jail.local file. maxretry = 3: If the attacker fails with this setting, he will be blocked for three times. findtime = 600: This is the observation period in 10 minutes. bantime = 600: If the attacker fails, the attacking address will be blocked for 10 minutes. This helps to reduce repeated attempts to log in.

<img width="940" height="618" alt="image" src="https://github.com/user-attachments/assets/d87017d6-df4b-434d-a052-3b7e6a582fc2" />
Fig9 - Confirming the ban
The first status output is for SSH jail, which is active and has no banned addresses. The second output indicates that there were four total failed logins and 10.10.1.20 is one of the banned IP addresses after multiple failed login attempts from the Bastion host. This will show that fail2ban has noticed the repeated failures and has applied the ban as set in the configuration file.

<img width="940" height="640" alt="image" src="https://github.com/user-attachments/assets/2f1d125b-594c-4fb2-8c45-d5b09846c2f1" />
Fig10 - SSH tunnel testing
The Bastion host was the first host to be authorised as an Admin key. The command ssh -f -N -L 9090:10.10.1.40:8080 root@10.10.1.20 then created a local SSH tunnel from Admin port 9090, through the Bastion, to the Internal host’s web service on port 8080. The curl http://localhost:9090/ result was: <h1>Internal Server</h1> which is a success message that the internal service was accessed successfully via the tunnel.

<img width="1919" height="916" alt="image" src="https://github.com/user-attachments/assets/02c2bc8e-bdf4-4e94-9ee7-cf6027ad1dbe" />
Fig11 - Internal-link packet capture
This capture displays TCP traffic between Bastion 10.10.1.20 and Internal host 10.10.1.40 on port 8080, and HTTP traffic. This is because the entire HTTP request and response have escaped the encrypted SSH tunnel at the Bastion. This verifies that the Bastion set up the end-to-end connection to the Internal web service.
<img width="1919" height="904" alt="image" src="https://github.com/user-attachments/assets/748a4271-e338-4754-bf3b-a8336fd6c990" />
Fig12 - Admin-link packet capture
This is a system monitoring capture of the SSH traffic between Admin 10.10.1.10 and Bastion 10.10.1.20 on port 22. The packets are labelled as "encrypted SSH" traffic, and therefore the content of the HTTP request or webpage cannot be deciphered along this link. This indicates that the request was encrypted as it goes to and from Bastion to Admin.







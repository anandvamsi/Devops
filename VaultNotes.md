# Vault
- What is Need of Ansible vault.
- How to encrypt file using ansible-vault.
- How to view an encrypted file using ansible vault.
- How to edit encrypted files.
- How to change the password of encrypted files.
- How to decrypt the encrypted files.
- Using the Ansible vault password file.


## Need of Ansible vault.
Ansible Vault is a tool designed to address the need for ```secure handling of sensitive information, such as passwords, API keys, and other credentials, within Ansible playbooks and roles```. It provides encryption for these sensitive data, ensuring that they are securely stored and transmitted
- Security
- Compliance
- Automation.

## How to encrypt a string and a file using ansible-vault.
To encrypt a string
```bash
ansible-vault encrypt_string 'mydbpassword' --name 'password_var'
New Vault password:
Confirm New Vault password:
password_var: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          32303434616165616230326139613938313761643539663232623261376634326634613439653065
          3866383663663032626435323835646538646330643062650a316137363461333032666539383938
          30653332366566333964313338326234313134366166303165313563653662383430373835363061
          3835666636346364660a663132353965343964623834633465356265396232613939356661343731
          6639
Encryption successful
```
- To encrypt a file
```bash
ansible-vault encrypt data.config
New Vault password:
Confirm New Vault password:
Encryption successful
anand@PF39MKQN:~/ansible$
anand@PF39MKQN:~/ansible$ cat data.config
$ANSIBLE_VAULT;1.1;AES256
39623634366666343332383732616336376161623564356362396566313438623535366563373239
3133623164666239666166396565643435356433303363660a316633323038323662663436316266
61373638333330663166383831663961343363636339363638346535666332316136666163393439
6364666334643132380a356136376130373766326235623136643566663836653761646238373430
61636135386132353032383465653966383836353735386233383035396434666533
```
- To decrypt a file.
```bash
 ansible-vault decrypt data.config
Vault password:
Decryption successful
anand@PF39MKQN:~/ansible$ cat data.config
This is a secret key file
```

## Encryption using .vaultpass file
- Note: .vaultpass should have the vault password
```bash
ansible-vault encrypt --vault-password-file .vaultpass  data.config
Encryption successful
anand@PF39MKQN:~/ansible$
anand@PF39MKQN:~/ansible$ cat data.config
$ANSIBLE_VAULT;1.1;AES256
64376132646362356235336238373162303934663236313931613931373362633463373866386134
6461343939376533663730343539316263626534343261660a303764636334326633366630353432
64363938346133363661623966343739613335386566336461306631313266363939336237393864
6233306539313362650a323531323133636430396264323231316530336633623933613663666365
39346165393839373033386564373863623336396632653562306166396433383738
```
- To descrypt the file
```bash
ansible-vault decrypt --vault-password-file .vaultpass  data.config
Decryption successful
anand@PF39MKQN:~/ansible$ cat data.config

This is a secret key file
```

##  How to edit the secret
```bash
ansible-vault edit --vault-password-file .vaultpass  data.config
anand@PF39MKQN:~/ansible$ cat data.config
$ANSIBLE_VAULT;1.1;AES256
61393364616638666466633935333331343233336439313636616666646166643164623531653463
6435326531363831333561306665323134346634353566620a376266396465326465643332646230
37303738663038623933383265663637666434333238343038613233383430663237663262353865
3465313731363731330a356338656337396461353537353461616432366263306464303135636664
64356239636632366264643730373963363964646664386564313561663163613764646530343464
3762373038363734643536393337373036376432343963393536

anand@PF39MKQN:~/ansible$ ansible-vault decrypt --vault-password-file .vaultpass  data.config
Decryption successful
```




This feature of Ansible allows you to keep your sensitive data encrypted like passwords and keys.
• Ansible provide a command line tool ansible-vault for edit sensitive files.
• When you run a playbook then command line flag -ask-vault-pass or -vault-password-file can be used.
• Vault can encrypt any structured data file used by Ansible.


Method1:- To encrypt string
> Encyrpting a string
ansible-vault encrypt_string --vault-password-file .vaultpass 'foobar' --name '<variable>'


Method2:- To encyrt string
ansible-vault encrypt_string
New Vault password:
Confirm New Vault password:
Reading plaintext input from stdin. (ctrl-d to end input)
rajesh
!vault |
          $ANSIBLE_VAULT;1.1;AES256
          64353764313538363463313931393232396630353466393638383436626231313161393130636139
          35643631383362326539353262326462653533360a373333386665313234376534303964
          61613132353033386637633036XX346630653463646330363237396132626233666334396334643536
          3761353133646439340a346335656364636136386365343233626237333631623765636661393362
          6233
Encryption successful

>> Above will encrypt string "rajesh"

---------------------------------------------------------------------------------------------------------


##To encrypt a new file 
 ansible-vault create mysecretfile
 :- This will prompt vault password

##To edit the secret
ansible-vault edit <mysecretfile>
 :- This will open the mysecretfile in vim editor

 #To view the password in cli
 ansible-vault view mysecretfile


 When you want to rotate the vaultpassword
  ansible-vault rekey mysecretfile
Vault password:
New Vault password:
Confirm New Vault password:
Rekey successful



> Method 1
#Encrypting the file:- This will encrypt already existing file
ansible-vault encrypt  roles/nonvault/vars/main.yml

#Executing the play Decoding happens
ansible-playbook main.yaml   --ask-vault-pass




>>Method 2
#Using VaultPassword file
cat .vaultpass
01123

#encrypting the file using vault-password-file
ansible-vault encrypt vaultfile/files/db_data --vault-password-file .vaultpass

#Executing the play with password file
ansible-playbook  main.yaml --vault-password-file .vaultpass

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


## Change the password of encrypted files
```bash
ansible-vault rekey  data.config
Vault password:
New Vault password:
Confirm New Vault password:
Rekey successful
anand@PF39MKQN:~/ansible$ cat data.config
$ANSIBLE_VAULT;1.1;AES256
62393137343264303266363265326331613065613930376430366563333166313766386361356366
3264396434366638343862623331373138616462623033320a356563343332623766333636393932
39623133653834653637646162386334336165313231663138353237303431366437336537656561
6238366464616564310a356236633865643934626162366136353031356561623966383930366561
31373038663438663133646534346436666430336432366331646434383563653463306662663038
6330363233356262303238353334626161313530313363643766
```

## How to execute ansible playbook 
```bash
ansible-playbook -i ansible/inventory sample-playbook.yaml –ask-vault-pass
```


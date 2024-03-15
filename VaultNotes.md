# Vault:-
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

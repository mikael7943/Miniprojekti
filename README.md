# Kehitysympäristön automaattinen asennus ansiblella. 
### Tekijät: Jaakko Saikkonen & Mikael Kurvinen
## Mihin tarkoitukseen
- Saadaan tietyt paketit sekä ohjelmat asennettuna kerralle uudelle järjestelmälle
- Valmis "starter pack" uusille työntekijöille
- Ohjelma asentaa seuraavat ohjelmat:
   - Git
   - Curl
   - Vim
   - Docker
      - Antaa mahdollisuuden rakentaa "kontteja" jotka toimivat samalla tavalla kuin virtuaalikoneet             mutta luovat vain "hiekkalaatikkomaisen" ympäristön ei koko konetta  
   - Node.js
      - Antaa kehittäjille mahdollisuuden luoda ja käyttää Javascript koodia
   - npm
      - Node Package Manager, jolla hallitaan JavaScript-paketteja

## Projektin esivalmiudet
- git asennettuna master koneelle
  https://git-scm.com/install/linux
- ansible asennettuna master koneelle
  https://terokarvinen.com/hello-ansible/?fromSearch=ansible
- tiedossa agent koneen IP osoite ja koneet saavat  _ping_ ja _ssh_ yhetyden toisiinsa
  - IP osoitteen saa selville kun antaa komennon **_ip a_** 
## Miten asentaa
Kloonaa tämä repositio seuraavalla comennolla 
```
git clone git@github.com:mikael7943/Miniprojekti.git
```
Kun kloonaus on onnistunut muista tarkistaa että **_inventory.ini_** tiedostossa on agent koneen ip osoite asetettuna oikein 


Mikäli haluat kokeilla ajaa playbookin toisella virtuaalitietokoneella pitää sinun tehdä seuraavat askeleet:

1.	Vaihda master-tietotokeen ja agent-tietokoneen verkkoasetukset virtualboxista.
      - Adapter 1: laita Host-only Adapter
  	   - Adapter 2: NAT

3.	Hae agent-tietokoneen ip-osoite komennolla ip a.

4.	Asenna ssh agent-tietokoneelle ja kokeile toimivuus.
      - https://terokarvinen.com/ssh-public-key-login-without-password/

6.	Konfiguroi agent-tietokone, että sudo toimii ilman salasanaa
      - https://terokarvinen.com/passwordless-sudo/?fromSearch=password

7.	Luo ssh-avainpari ja kopioi se, jotta master-tietokone pääsee kirjautumaan ilman salasanaa agent-tietokoneelle.

8.	Lisää agent-tietokone inventory.ini tiedostoon master-tietokoneella. 

9.	Voit kokeilla toimivuutta ping komennolla ansible -i inventory.ini dev -m ping

10.	Aja komento ansible-playbook -i inventory.ini playbook.yml

## Projektin rakenne
Koko ansiblen rakenne:  
<img width="522" height="341" alt="image" src="https://github.com/user-attachments/assets/d2702a73-d1e3-49ff-9eb2-cab7fee786b2" />  

---

### /roles/common/tasks/main.yml
- Suurin osa perusohjelmien latauksesta tapahtuu täällä
- Päivittää pakettivaraston
```YAML
- name: update apt cache
  apt:
    update_cache: yes

- name: Install basic packages
  apt:
     name:
       - git
       - curl
       - vim
     state: present
```
---
### /roles/docker/tasks/main.yml
- Asennetaan docker sen viimeisimpään versioon
- Käynnistetään docker ja laitetaan se käynnistymään aina koneen mukana
- Lisätään käyttäjä docker ryhmään jotta pystyy ajamaan docker komentoja ilman sudo oikeuksia

  ```YAML
  - name: Install Docker
    apt:
      name: docker.io
      state: present

  - name: Start Docker service
    service:
      name: docker
      state: started
      enabled: true

  - name: Add user to docker group
    user:
      name: "{{ ansible_user_id}}"
      groups: docker
      append: yes

  ```
---
### /roles/node/tasks/main.yml
- Asentaa node.js
- Asentaa npm
```YAML
- name: Install Node.js
  apt:
    name: nodejs
    state: present
- name: Install npm
  apt:
    name: npm
    state: present

```
## Ansiblella ajot ja projektin toimivuuden testaus

- Ensimmäinen ansible ajo lataa kaiken halutun target-koneelle  

<img width="522" height="341" alt="Näyttökuva 2026-05-05 215307" src="https://github.com/user-attachments/assets/13d16370-ad13-4250-8e9d-3d844f87233b" />  

---

- Toisella ajolla tarkistetaan idempotenssi  

<img width="522" height="341" alt="Näyttökuva 2026-05-05 215334" src="https://github.com/user-attachments/assets/2bb19437-ab7c-4c68-9b68-2601769fcf76" /> 

---

- Testaus target-tietokoneelle, että toimiiko asennetut ohjelmat  

<img width="522" height="341" alt="Näyttökuva 2026-05-05 215726" src="https://github.com/user-attachments/assets/490ca39c-6588-4886-8910-5ef8da162ecb" />  

---

## LÄHTEET:

https://terokarvinen.com/

https://docs.ansible.com/

https://wiki.debian.org/SivuHaku

https://docs.docker.com/get-started/

Ansiblen ajossa toiselle virtuaalikoneelle käytimme copilottia ongelmien ratkaisussa



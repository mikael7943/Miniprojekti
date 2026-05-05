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
- ansible asennettuna master koneelle
- tiedossa agent koneen IP osoite ja koneet saavat  _ping_ ja _ssh_ yhetyden toisiinsa
## Miten asentaa
Kloonaa tämä repositio seuraavalla comennolla 
```
git clone git@github.com:mikael7943/Miniprojekti.git
```
Kun kloonaus on onnistunut muista tarkistaa että **_inventory.ini_** tiedostossa on agent koneen ip osoite asetettuna oikein 


Mikäli haluat kokeilla ajaa playbookin toisella virtuaalitietokoneella pitää sinun tehdä seuraavat askeleet:

1.	Vaihda host-tietotokeen ja target-tietokoneen verkkoasetukset virtualboxista.
   Adapter 1: laita Host-only Adapter
   Adapter 2: NAT

2.	Hae target-tietokoneen ip-osoite komennolla ip a.

3.	Asenna ssh target-tietokoneelle ja kokeile toimivuus.

4.	Konfiguroi target-tietokone, että sudo toimii ilman salasanaa

5.	Luo ssh-avainpari ja kopioi se, jotta host-tietokone pääsee kirjautumaan ilman salasanaa target-tietokoneelle.

6.	Lisää target-tietokone inventory.ini tiedostoon host-tietokoneella. 

7.	Voit kokeilla toimivuutta ping komennolla ansible -i inventory.ini dev -m ping

8.	Aja komento ansible-playbook -i inventory.ini playbook.yml

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

---

Ensimmäinen ansible ajo lataa kaiken halutun target-koneelle
<img width="522" height="341" alt="Näyttökuva 2026-05-05 215307" src="https://github.com/user-attachments/assets/13d16370-ad13-4250-8e9d-3d844f87233b" />  
Toinen ajolla tarkistetaan idemotenssi
<img width="522" height="341" alt="Näyttökuva 2026-05-05 215334" src="https://github.com/user-attachments/assets/2bb19437-ab7c-4c68-9b68-2601769fcf76" />  
Testaus target-tietokoneelle, että toimiiko asennetut ohjelmat
<img width="522" height="341" alt="Näyttökuva 2026-05-05 215726" src="https://github.com/user-attachments/assets/490ca39c-6588-4886-8910-5ef8da162ecb" />  



LÄHTEET:
https://terokarvinen.com/

https://docs.ansible.com/

https://wiki.debian.org/SivuHaku




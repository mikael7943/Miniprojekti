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
      - Antaa mahdollisuuden rakentaa "kontteja" jotka toimivat samalla tavalla kuin virtuaalikoneet mutta luovat vain "hiekkalaatikkomaisen" ympäristön ei koko konetta  
   - Node.js
      - Antaa kehittäjille mahdollisuuden luoda ja käyttää Javascript koodia

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
## Projektin rakenne
Koko ansiblen rakenne:  
<img width="522" height="341" alt="image" src="https://github.com/user-attachments/assets/d2702a73-d1e3-49ff-9eb2-cab7fee786b2" />  

<img width="522" height="341" alt="Näyttökuva 2026-05-05 215307" src="https://github.com/user-attachments/assets/13d16370-ad13-4250-8e9d-3d844f87233b" />  

<img width="522" height="341" alt="Näyttökuva 2026-05-05 215334" src="https://github.com/user-attachments/assets/2bb19437-ab7c-4c68-9b68-2601769fcf76" />  

<img width="522" height="341" alt="Näyttökuva 2026-05-05 215726" src="https://github.com/user-attachments/assets/490ca39c-6588-4886-8910-5ef8da162ecb" />  




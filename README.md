# π¦ openmeetings νκ²½ μ€μ  κΈ°λ‘

## μ¬μ©λ²
1. `docker` μ `docker-compose` ν¨ν€μ§κ° νμνλ€. λ°λΉμ/λ¦¬λμ€ κΈ°μ€
```bash
  sudo apt install docker docker-compose -y
```
2. νκ²½λ³μ `EXTERNAL_IP` λ₯Ό μ€μ ν  νμκ° μλ€. νΉμ, μ΄ λ ν¬ ν΄λ‘  λ£¨νΈ ν΄λμ `.env` νμΌμ λ§λ€μ΄μ λ€μκ³Ό κ°μ΄ μ μ΄μ£Όμ
```.env
EXTERNAL_IP=[μΈλΆμμ μ΄ μλ²μ μ κ·Ό κ°λ₯ν μμ΄νΌ μ¬κΈ°μ μ κΈ°]
```
3. μ΄ λ ν¬ ν΄λ‘  λ£¨νΈ ν΄λμ `./data/coturn` ν΄λ μμ±
4. ν΄λΉ ν΄λ μμ `turnserver.conf` μμ± (μμ λ μλ λ©λͺ¨μ μμ)
5. `sudo openssl rand -hex 32` λ₯Ό μλ ₯νλμ§ μλλ©΄ μλ λ©λͺ¨ ννΈμ μ€ν¬λ¦½νΈλ₯Ό λΈλΌμ°μ  μ½μμμ μ€νν΄μ λλ€ν μμ ν 32λΉνΈ λΉλ°λ²νΈλ₯Ό λ§λ€μ΄μ€λ€.
6. `turnserver.conf` νΈμ§
  - `#use-auth-secret` -> `use-auth-secret` μ£Όμ ν΄μ 
  - `#static-auth-secret=north` -> `static-auth-secret=[μ’μ μ μμ±ν 32λΉνΈ λΉλ°λ²νΈ]` μ£Όμ ν΄μ  ν static μνΈ μλ ₯
  - `#realm=mycompany.org` -> `realm=[λλ©μΈ νΉμ μμ΄νΌ]` μ£Όμ ν΄μ  ν μλ² λλ©μΈ νΉμ μμ΄νΌ μλ ₯
  - `#stale-nonce=600` -> `stale-nonce=0` nonce μλͺμ 0μΌλ‘ μ€μ ν΄ ν΄λΌμ΄μΈνΈ μΈμ¦μ΄ λ§λ£λμ§ μλλ‘ μ€μ 
7. λΉλλ openmeetings ν΄λ μμμ*(μ€λͺμΆκ°νμ) `./webapps/openmeetings/WEB-INF/classes/openmeetings.properties` νμΌμμ λ€μ λΌμΈ μμ 
  - `kurento.turn.url=` -> `kurento.turn.url=[λλ©μΈ νΉμ μμ΄νΌ]:3478`
  - `kurento.turn.secret=` -> `kurento.turn.secret=[μ’μ μ μμ±ν 32λΉνΈ λΉλ°λ²νΈ]`
8. λ£¨νΈ ν΄λ μμμ λ€μ λͺλ Ήμ΄λ‘ μμ
```bash
  docker-compose up -d
```

## λ©λͺ¨
- ./data/coturn/turnserver.conf μμ 
  - https://github.com/coturn/coturn/blob/master/examples/etc/turnserver.conf
- λΈλΌμ°μ  μ½μμ μ΄μ©ν΄μ μμ ν 32λΉνΈ λλ€ λΉλ°λ²νΈ μμ±
```javascript
(function(){
  var buf = new Uint8Array(32);
  window.crypto.getRandomValues(buf);
  console.log(Array.prototype.slice.call(buf).map((v) => v.toString(16).padStart(2, '0')).join(''));
})();
```

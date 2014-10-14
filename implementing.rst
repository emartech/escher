Implementation Guide
====================

Implementing the Escher protocol is not trivial. We have collected a strategy,
best practices and ideas we think will help you if you would like to create an
Escher implementation in a new programming language.

1. hozzon létre egy olyan osztályt, ami kezelni tudja a különböző requesteket
különböző programnyelveken kül környezetekben másképp állnak rendelkezésre a http requestek
hasznos ha az srnek van egy saját reprezentációja függtln az adat framework request osztályától

ide példák (külön oldal)

2. megvannak a részek a fejlécből meg a requestből, és ebből elő kell állítani a kanonizált reprezentációt

3. ha megvan a kanonizált request, abból előállítani a signiture-t

4. ha rendelkezésre áll a signiture, akkor hozzáadni a requesthez egy presigned URL-hez

1 oldal, ahol elmagyarázzuk a beállítási lehetőségeket
idő:aktuális ideje a könyvtárnak, az alapján ellenőrzi h valós-e a kérés. be lehet állítani h mi a valós idő (így tesztelhető)

 * Create a group which can handle all requests
    * in a particular programming language and setting, http requests are available
      differently
    * it is useful if the SR has a representation independent of the information's framework request group
 * After you've created the parts of the header and request, you need to generate the canonical representation
 * Create the signiture from the canonical request
 * If the signiture is disposable, you can add it to a presigned URL to the request

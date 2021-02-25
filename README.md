# SSH-DISPLAY
Projecte SSH x11forwarding

La idea d'aquest projecte naix de la necessitat d'utilitzar la instrucció ssh amb la redirección de pantalla (X11 forwarding).

El projecte es basa en un conjunt d'ordinadors client i un sol ordinador servidor que controla totes les pantalles dels ordinadors client.

Amb la idea del projecte clara el que vam pensar va ser el crear un joc que utilitza tots els requeriments.
D'aquesta manera vam pensar en crear el típic joc "simón dice" (que es tracta de una sentencia ja siga lletres paraules o sons i una persona o conjunt de persones tenen que tractar de reproduir exactament el que diu la sentencia) aleshores vam veure que utilitzant aquestes pantalles podriem mostrar números per les diverses pantalles per a que així el que tinga el ordinador servidor vega la pantalla dels ordinadors client i introduixca el ordre en el script i si l'ordre és correcta que pase al següent nivell.

Ja teníem clar el projecte, sols ens faltaba aplicar-lo utilitzant scripts.

Aleshores es descarreguem el .zip en el directori /home/$USUARI/
I el desempaquetarem en aquest directori.

Pasos:
1. Configurar els clients
2. Configurar el servidor amb els clients
3. Crear els enllaços dels clients amb el servidor
4. Copiar els fitxers
5. Fem una prova de funcionament
6. Iniciar el joc

1. Configurar els clients.

Deguem crear el usuari test en tots els ordinadors client per a tindre més comoditat a l'hora de fer connexions, ja que seria el mateix usuari per a tots.

A continuació el que farem sera crear les claus en els clients.Per a fer aço hi ha que tindre instal·lat ssh en els ordinadors.

Per a instal·lar ssh:
$ sudo apt-get install openssh-server
Una vegada instal·lat el que farem sera crear les claus.
$ ssh-keygen
I deixem totes les respostes per defecte, utilitza la tecla de [Intro]

També hi ha que asegurarse de que tinga instal·lat firefox els nostres clients, per al correcte funcionament del joc.



2. Configurar el servidor amb els clients.

En el fitxer /etc/hosts es troben tots dominis assignats manualment a les ip's. Aleshores afegirem en el ordinador del servidor, les ips dels clients amb relació amb els dominis corresponents.

Com volem que tinga cada client un número, el que farem en el fitxer de hosts sera introduïr el número que vuigam amb la ip del client corresponent a ixe número, per a que alhora d'executar el script es mostre el número en el client corresponent. Es a dir
#/etc/hosts
[ip_client_1]    one
[ip_client_2]    two
…
Per exemple la ip del client 1 sera 10.20.30.1 i la del 2 10.20.30.2
10.20.30.1    one
10.20.30.2    two

3. Crear els enllaços.

I després de configurar els dominis afegiriem les claus amb el que hem configurat anteriorment.Per a fer açò el que hem utilitzat ha sigut la instrucció.

$ ssh-copy-id [usuari_client]@[domini_client]

En el nostre cas per a l'ordinador que hem decidit que siga el número 1 amb el domini one i el usuari test, aleshores es completaria així.

$ ssh-copy-id test@one

I repetirem aquest comandament per a cada usuari amb el seu domini corresponent.

4. Copiar els fitxers.

El joc funciona en fitxers html i s’obri a pantalla completa en el navegador firefox.
D'aquesta manera , necessitem que cada client tinga els fitxers de html corresponents.

Per a copiar els fitxers del servidor al nostre client uilitzem el següent comandament:

$ scp -r [usuari_servidor]@[ip_servidor]:/home/$USUARI_SERVIDOR/SSH-DISPLAY/html /home/$USUARI_CLIENT/

Hi ha que asegurarse de que el directori que continga els fitxers html en el client estiga en la carpeta html dins del directori personal de test, per lo tant els fitxer tindrien que estar en /home/test/html. Per al correcte funcionament del script




5. Fem una prova de funcionament.

Una vegada tot configurat i enllaçat fem una prova de connexió. Fem ssh als respectius clients desde el servidor:

$ ssh [usuari_client]@[domini_client]

Per exemple per a conectarse al client número 1:

$ ssh test@one

I en teoria no hauria de demanar ningun tipus de confirmació, tindria que accedir automàticament(si està tot ben configurat, si no, caldrà repetir els passos anteriorment mencionats).
Per a saber que el joc funcionaria correctament executem el script testpantalla.sh que el que farà serà obrir les pàgines corresponents en els ordinadors respectius, és a dir en el ordinador que tenim configurat el domini one mostrarà per pantalla el numero 1, de la mateixa manera amb els demés ordinadors i els seus dominis. 

Per a que obriguen tots els números corresponents el que tinguem que fer es que quan es mostra per pantalla el número en el domini del client corresponent, per a que es mostre el següent número en el següent ordinador hi ha que donar-li a les tecles [Ctrl] + [C] per a així que execute el script la següent ordre i poder fer funcionar correctament el joc.

Depenent de la quantitat de clients que tingues, tindras que modificar mes o menys linies en el script.

6. Iniciar el joc.

Una vegada tinguem tots els ordinadors clients enllaçats correctament amb el servidor en les claus relacionades, el fitxer /etc/hosts configurat, els fitxers html copiats en tots els client i hem executat el script de testpantalla.sh, ja podem executar el joc. 

El funcionament d’aquest joc es basa en un número aleatori acotat al número de clients que vuigues utilitzar.
El script obri mitjançant ssh el fitxer html(corresponent al número de domini de cada client)  en el navegador firefox a pantalla completa. Cada vegada que es mostre un numero per pantalla en els ordinadors clients el que farà serà esperar a que introduïsques el ordre corresponent en el que han anat apareixent aquests números i d’aquesta manera si has introduït el ordre correcte pasaras al següent nivell.

En el moment que falles, introduïnt l'ordre, et demanarà que introduisques el teu nom per a tindre registrat fins a quin nivell has arribat, per a posteriorment mostrar un ranking dels 5 millors.

I ja estaria tot

Hi ha que tindre en compte quants ordinadors clients anem a tindre per a així modificar exactament els scripts corresponents.

El script score.sh funciona per a veure el rànquing dels 5 millors.

El fitxer log.txt es ahon es troben totes les dades que llegira el script score.sh



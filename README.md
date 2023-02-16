# h9 Sequel

x) Yrityssoftaa. Keksi esimerkki palvelusta, jota käytetään wepissä selaimella, koodi ajetaan palvelimella ja taustalla on tietokanta. Mitä etuja tällä toteutustavalla on vaihtoehtoisiin toteutustapoihin verrattuna? (Tässä x-alakohdassa ei tarvitse tehdä testejä koneella tai toteuttaa mitään, pelkkä kuvittelu ja vastauksen kirjoittaminen riittää)
a) Postgre. Asenna PostgreSQL ja testaa se suorittamalla SQL-komento. (Jos teit jo tunnilla, tee uusi Linux-käyttäjä ja tälle tietokanta ja tietokantakäyttäjä.)
b) Crud. Kokeile CRUD (create, read, update, delete) kirjoittamalla SQL-käsin. (Artikkeli alla vinkeissä kertoo, kuinka tämä tehdään. Jos SQL on tuttua, voit keksiä tauluille ym omat aiheet ja nimet.)
n) Vapaaehtoinen: Maria. Asenna MariaDB ja kokeille sillä CRUD.

## a)

- Asensin Postgre:n jo edellisellä tunnilla, joten loin uuden käyttäjän.
- Komento: ``$ sudo adduser miikka2``, seurasin komentokehotteen ohjeita. Syötin salasanan sekä kohtaan Full Name: Test user for Miikka.
- Annoin käyttäjälle sudo oikeudet ``$ sudo adduser miikka2 sudo``
- Kirjauduin uudella käyttäjällä sisään ja käynnistin sillä ``postgresql`` komennolla ``$ sudo systemctl start postgresql``
- Loin uuden tietokannan uudelle käyttäjälleni komennolla ``$ sudo -u postgres createdb miikka2``
- Loin uuden tietokantakäyttäjän komennolla ``$ sudo -u postgres createuser miikka2``

![Add file: Postgres](postgres-db-user.png)

- Komennolla ``$ psql`` avasin postgresql:n.
- Testasin, että SQL-komennot toimii luomalla tyhjän taulun tietokantaan. Loin taulun lausekkeella: ``CREATE TABLE testi();`` ja katsoin onnistuiko taulukon luominen syöttämällä ``\d``

![Add file: SQL testi](sql-testi.png)

- Taulukon luominen onnistui eli SQL-komennot toimii.

- Komennolla ``\d books;`` näkee taulukon rakenteen, tässä näkyy esimerkiksi luomani ``PRIMARY KEY id``, sekä ``name`` sarake sekä niiden tyypit integer/numero, character varying(200)/max 200, merkkijono, sekä esim id:n määritelmät/ehdot. ``not null``= ei voi olla null arvo ja Defaultissa määritellään, että tietokanta luo automaattisesti uuden id:n esimerkiksi, kun luodaan taulukkoon uusi kirja.

## b)

- Poistin a) kohdassa testiksi luomani taulukon tietokannasta lausekkeella ``DROP TABLE test;``. ``DROP TABLE`` lausekkeen alla kertoo, että komento toteutui. Tarkistin vielä ``\d``, vastaukseksi tuli "Did not found any relations" eli taulukko poistui.

### CREATE

- Loin uuden taulukon kirjoille lausekkeella ``CREATE TABLE books (id SERIAL PRIMARY KEY, name VARCHAR(200));
  - SERIAL id luo automaattisesti numeraalisen id:n.
  - name VARCHAR(200), luo "name" sarakkeen, joka on max 200 merkkinen merkkijono.
- Katsoin luomani taulukon komennolla``\d``, taulukon luominen onnistui.

![Add file: Create Table](create-table.png)

- Seuraavaksi syötin hieman tietoja juuri luomaani "books" taulukkoon. 
- Syötän taulukkoon muutaman päästä keksimäni nimen komennolla ``INSERT INTO books(name) VALUES('nimi')``;

### READ

- Katsotaan tulivatko nimet näkyviin taulukkoon komennolla ``SELECT * FROM books;``

![Add file: Select](select-books.png)

- Kyllä vain, nimien lisääminen onnistui.

- SELECT lausekkeisiin voi sisällyttää ehtoja, jotka helpottavat merkittävästi isoista tietokannoista haluttujen asioiden hakemista. Tässä massiivisessa tietokannassa hain kirjojen nimistä "tie" sanaan loppuvaa kirjan nimeä komennolla ``SELECT * FROM books WHERE name LIKE '%tie'``. Lausekkeessa ``%`` ennen "tie" sanaa määrittelee, että "tie" sanaa ennen voi olla mitä vain merkkejä.

#### Tulos:

![Add file: Select](select-where.png)


### UPDATE

- Päivitin "Kärsimysten tie" kirjan nimen lausekkeella ``UPDATE books SET name='Via dolorosa, kärsimysten tie' WHERE name='Kärsimysten tie';``
- Katsotaan jälleen päivittyiki nimi taulukkoon: ``SELECT * FROM books;``

![Add file: update](update-book.png)

- Kyllä päivittyi.

### DELETE

- Kirjan poistaminen luonnistuu lausekkeella: ``DELETE FROM table WHERE name='nimi'``. 
- Ensin listasin kirjat ``SELECT * FROM books;``
- Poistin kirjan: ``DELETE FROM books WHERE name='Älä poista minua'``, ``DELETE 1`` kertoo, että lauseke suoritettiin.
- Varmistin, että kirja poistui listaamalla kirjat uudelleen.

![Add file: delete](delete-book.png)

- Kirjaa ei löydy enää taulukosta eli poistaminen onnistui.

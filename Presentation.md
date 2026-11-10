# Brief — Automatiser sa recherche d'emploi Data Analyst avec Make (version JSearch) 

 

Mon projet permet de regrouper les offres d'emploi de partout sur le web via l'API OpenNinja JSearch et d'automatiser leurs ajouts sur un Google spreadsheet, afin d'optimiser ma recherche d'offres et mes dépôts de candidatures. Cette opération est paramétrée en no code via Make et réglée pour être rafraichie tous les vendredis à midi. En même temps, ces alertes emplois apparaissent sur un salon discord privé via un webhook. 

  

Mes outils sont : Google docs (sheets), Make, OpenWeb Ninja API JSearch, Discord et Make. 

 

Mon scénario Make est composé de 5 modules :  

Un module HTTP GET : c'est lui qui fait les demandes d'information auprès de l'API, avec ma clef API. Il cherche les offres pour les mots clef "data analyst, nord, pas de calais", en France, par mois, au moins pour une première requête (car il y en avait trop peu dans la région en filtrant par semaine) 

- Un module iterator: répète l'action pour obtenir toutes les nouvelles occurrences d'informations (offres d'emplois),  

- Un module Google Sheets Search Rows : existe pour filtrer les doublons, avec le filtre intermodule entre lui-même et Google Sheets Add a Row : il compare le module Google Sheets Add a Row avec les entrées que lui envoie l'iterator. S'il constate qu'un lien pour postuler existe déjà dans le sheet, il écarte la ligne entrante de l'itérator entièrement, 

- Un module Google Sheets Add a Row : c'est un "récipent", une cible qui récupère les informations demandées. Toutes les colonnes sont personnalisables. C'est là que je vais consulter le détail des offres et ajouter mes notes, 

- Un module HTTP POST : lui s'occupe de poster un aperçu (également personnalisable) des nouvelles offres d'emploi importées sur un salon discord, via un webhook. De cette manière, je recevrai des push notifications sur mon compte discord dès que le rafraichissement s'opèrera les vendredis midi. 

 

Pour faire tout mon setup, et tester plusieurs fois que tout fonctionnait (ou pas, et réparer), j'ai utilisé moins d'un quart de mes crédits mensuels Make et 7% de mes crédits OpenWeb Ninja.  

D'après mon historique Make, mon scénarion coûte 6 crédits à lancer manuellement, sans utilisation de webhook. Il y a donc largement de quoi faire avec 1000 crédits par mois (estimation : 6 crédits x 4 semaines dans le mois, +  les impressions webhooks = 24 + ? crédits sur un total mensuel de 1000). 

Pour JSearch, s'il ne reçoit que 1 demande par semaine, cela fera 4 par mois, sur un total de 200 requêtes mensuelles possibles. Là encore, je n'utiliserai pas tout avec ce scénario. 

 

Une difficulté rencontrée :  

Mon premier résultat m'a donné 10 offres différentes dans mon gsheet, et 10 pings identiques dans discord. Je fais maintenant attention à la source que je sollicite : le gsheet s'adressait (comme il fallait le faire) à l'iterator, tandis que le HTTP POST s'adressait au HTTP GET.  

 

## Questions brief 

1 - Pourquoi l'API demande-t-elle une clé à chaque appel ? Que risquez-vous si votre clé se retrouve sur GitHub ? 

L'API demande une clef à chaque appel car elle ne la garde pas en mémoire, d'une requête à l'autre. 

Une clef agit comme un mot de passe. C'est donc une information sensible, à ne pas divulguer.  

 

2 - Un webhook est une « adresse postale » du salon. Pourquoi faut-il la garder secrète ? 

De la même manière qu'on ne voudrait pas recevoir la visite de tout le monde, y compris de personnes mal intentionnées qui voudraient tout casser / voler chez nous, il faut dissimuler le webhook pour éviter d'avoir des individus non sollicités sur le salon discord concerné. Ils auraient accès à la liste des participants du server discord également et pourraient @everyone et poster des liens de phishing etc. 

 

3 - A quoi sert la colonne « site d'origine » ? Quelles colonnes Make doit-il remplir, et lesquelles sont pour vous ? 

La colonne "site d'origine" répertorie le site de l'entreprise émettrice de l'offre. Ce n'est pas le lien de l'offre. Make doit remplir toutes les colonnes demandées, mais celle qui nous intéresse en premier lieu est celle qui contient les liens pour postuler.  La colonne "site d'origine" pourrait nous intéresser dans un second temps, si notre candidature est retenue, pour préparer un entretient. 

 

5 - Pourquoi certaines offres n'ont-elles pas de nom d'entreprise ou de lieu ? Que se passerait-il dans votre Sheet sans ifempty ? 

Certaines offres n'ont pas toutes les entrées demandées qui sont remplies. C'est un défaut d'écriture de l'offre. Sans ifempty, mon document comporterait des cases vides, ce qui pourrait gêner une comparaison inter-documents comme celle effectuée pour le bonus de ce brief.  

 

7 - Pourquoi une exécution par semaine avec date_posted = week évite-t-elle la plupart des doublons ? Combien de recherches différentes (villes, contrats) pouvez-vous lancer chaque semaine avec 200 requêtes JSearch et 1 000 crédits Make par mois ? 

Une annonce est moins susceptible d'être republiée d'une semaine à l'autre. Ainsi, dans une fenêtre de sept jours, les nouveaux imports sont a priori uniques. Un détail prévisionnel JSearch et Make est proposé en amont. Mon scénario actuellement requiert 6 crédits, sans utilisation de webhook. En supprimant mon HTTP GET, je pourrais lancer 166 requêtes différentes sur Make - ce qui n'atteindrait pas la limite des 200 requêtes JSearch mensuelles. 

 

Bonus - Questions guidantes : pourquoi ne pas chercher sur l'intitulé de l'offre ? Combien de crédits Make ce bonus ajoute-t-il par exécution ? 

J'ai choisi de chercher sur l'URL de l'annonce car c'est moins probable d'avoir une correspondance exacte (et donc un compte de doublon) sur un lien, que sur un intitulé d'offre. 

 

Je transmets l'URL de mon Google Sheet via Simplonline. 
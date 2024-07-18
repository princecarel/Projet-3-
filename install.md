# Synchronisation de GLPI avec Active Directory

Ce guide explique comment synchroniser GLPI avec Active Directory pour gérer les utilisateurs et les groupes.

## Étapes de Synchronisation

### 1. Connectez-vous à GLPI en tant qu'administrateur

### 2. Accédez au menu de configuration des Annuaires LDAP

- Allez dans Configuration -> Auth -> LDAP directories.

### 3. Ajoutez un nouvel annuaire LDAP

- Cliquez sur Ajouter pour ajouter un nouvel annuaire LDAP.

### 4. Configurez les paramètres de connexion LDAP

- Nom : Un nom descriptif pour l'annuaire (ex. Active Directory).
- Serveur LDAP :
  - Adresse IP ou nom de domaine : Adresse IP ou nom de domaine du serveur AD.
  - Port : 389 (par défaut) ou 636 pour une connexion sécurisée (LDAPS).
- Connexion :
  - DN de connexion : L'utilisateur avec les permissions nécessaires pour interroger AD (ex. CN=Admin,CN=Users,DC=domain,DC=com).
  - Mot de passe : Mot de passe de l'utilisateur.

### 5. Définissez les options LDAP

- BaseDN : Le point de départ dans l'annuaire (ex. DC=domain,DC=com).
- Filtre de recherche d'utilisateur : (objectClass=user).
- Attribut de l'utilisateur :
  - Connexion : sAMAccountName ou userPrincipalName.
  - Nom : cn.
  - Prénom : givenName.
  - Adresse de messagerie : mail.

### 6. Testez la connexion LDAP

- Cliquez sur Tester pour vérifier la connexion et les paramètres.

### 7. Enregistrez les paramètres

- Si le test est réussi, cliquez sur Ajouter pour enregistrer la configuration LDAP.

### 8. Configurez l'importation d'un utilisateur et autres utilisateurs

- Accédez à Configuration -> Utilisateurs
  Allez dans Configuration.
  Sélectionnez Utilisateurs.
- Importer les utilisateurs LDAP
  Cliquez sur Importer les utilisateurs LDAP.
- Sélectionnez l'annuaire LDAP
  Choisissez l'annuaire LDAP configuré précédemment.
- Définissez les filtres de recherche
  Filtre de recherche d'utilisateur : (&(objectClass=user)(sAMAccountName=paul.perin)(ou=chercheur)).
- Testez et importez
  Cliquez sur Tester.
  Si le test est réussi, cliquez sur Importer.
  
- Allez dans Configuration -> Users.
- Cliquez sur Importer les utilisateurs LDAP.
- Sélectionnez l'annuaire LDAP configuré précédemment.
- Définissez les filtres pour les utilisateurs à importer (ex. (objectClass=user)).

### 9. Planifiez l'importation automatique des utilisateurs

- Allez dans Administration -> Règles -> Import de l'utilisateur LDAP.
- Créez une nouvelle règle ou modifiez une règle existante pour automatiser l'importation des utilisateurs.
- Définissez les critères et les actions de la règle (ex. Si l'utilisateur existe dans LDAP, alors l'importer).

### 10. Testez l'importation des utilisateurs

- Exécutez manuellement l'importation pour vérifier que les utilisateurs sont importés correctement.

### 11. Synchronisation des groupes

- Allez dans Configuration -> Groups.
- Cliquez sur Importer les groupes LDAP.
- Sélectionnez l'annuaire LDAP configuré précédemment.
- Définissez les filtres pour les groupes à importer (ex. (objectClass=group)).

### 12. Testez la synchronisation des groupes

- Exécutez manuellement l'importation pour vérifier que les groupes sont importés correctement.

### 13. Configuration des droits des utilisateurs et des groupes

- Allez dans Administration -> Profils.
- Attribuez les droits appropriés aux utilisateurs et aux groupes importés selon les besoins de votre organisation.

### 14. Vérifiez les utilisateurs et les groupes importés

- Allez dans Administration -> Utilisateurs et Administration -> Groupes pour vérifier que tous les utilisateurs et groupes ont été importés correctement et que leurs informations sont à jour.

### 15. Automatisation de la synchronisation

Configurez une tâche cron pour automatiser l'importation et la synchronisation des utilisateurs et des groupes LDAP.

Exemple de tâche cron (à adapter selon votre environnement) :

```bash
# Créer un fichier de script d'importation GLPI
nano /usr/local/bin/glpi_ldap_sync.sh
```

Ajoutez le contenu suivant au script :

```bash
#!/bin/bash
/usr/bin/php /var/www/html/glpi/front/cron.php --force
```

Rendez le script exécutable :

```bash
chmod +x /usr/local/bin/glpi_ldap_sync.sh
```

Ajoutez la tâche cron pour exécuter le script régulièrement (par exemple, toutes les heures) :

```bash
crontab -e
```

Ajoutez la ligne suivante :

```bash
0 * * * * /usr/local/bin/glpi_ldap_sync.sh
```

Vous avez maintenant configuré GLPI pour synchroniser automatiquement les utilisateurs et les groupes depuis Active Directory. Pour plus d'informations, consultez la documentation officielle de GLPI.

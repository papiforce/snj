# Variables Forumactif — Référence complète (AwesomeBB)

## Variables globales (utilisables dans tous les templates)

### Forum

| Variable             | Description                |
| -------------------- | -------------------------- |
| `{SITENAME}`         | Nom du forum               |
| `{SITE_DESCRIPTION}` | Description du forum       |
| `{U_INDEX}`          | URL de l'accueil           |
| `{PAGE_TITLE}`       | Titre de la page en cours  |
| `{META_DESCRIPTION}` | Description meta SEO       |
| `{T_THEME_PATH}`     | Chemin du dossier thème    |
| `{T_IMAGESET_PATH}`  | Chemin des images du thème |
| `{TOTAL_POSTS}`      | Nombre total de messages   |
| `{TOTAL_USERS}`      | Nombre total de membres    |
| `{NEWEST_USER}`      | Dernier inscrit            |

### Navigation

| Variable              | Description                        |
| --------------------- | ---------------------------------- |
| `{U_REGISTER}`        | URL inscription                    |
| `{U_LOGIN_LOGOUT}`    | URL connexion/déconnexion          |
| `{L_LOGIN_LOGOUT}`    | Texte "Connexion" ou "Déconnexion" |
| `{U_PROFILE}`         | URL profil utilisateur connecté    |
| `{U_PRIVATEMAIL}`     | URL messages privés                |
| `{U_SEARCH}`          | URL recherche                      |
| `{U_VIEWONLINE}`      | URL membres en ligne               |
| `{U_FAQ}`             | URL de la FAQ                      |
| `{U_MCP}`             | URL du panneau de modération       |
| `{U_CP}`              | URL du panneau utilisateur         |
| `{GENERATED_NAV_BAR}` | Liens navbar configurés dans le PA |
| `{NAVBAR_BORDERLESS}` | Classe pour navbar sans bordures   |

---

## Variables utilisateur en AwesomeBB

> ⚠️ En AwesomeBB, les variables `{USERNAME}` etc. ne fonctionnent plus directement dans les templates. Elles doivent être appelées via JavaScript avec l'objet `_userdata`.

### Objet `_userdata` (JavaScript) — propriétés réelles

```javascript
_userdata = {
  // Identité
  username: "Admin",         // Pseudo
  user_id: 1,                // ID utilisateur (entier)
  user_level: 1,             // Niveau : 1 = admin fondateur, 2 = admin, 3 = modérateur, 4 = membre
  user_lang: "fr",           // Langue de l'interface
  user_posts: 1,             // Nombre de messages postés
  user_nb_privmsg: 0,        // Nombre de MP non lus
  groupcolor: "000099",      // Couleur hex du groupe (sans #)

  // Avatar
  avatar: '<img loading="lazy" src="https://..." alt="avatar" style="..." />', // HTML complet
  avatar_link: "https://...", // URL directe de l'image avatar

  // Session
  session_logged_in: 1,      // 1 si connecté, 0 si visiteur (⚠️ pas "is_logged_in")

  // URLs de navigation (déjà encodées, prêtes à l'emploi)
  page_home: "/forum",
  page_login: "/login",
  page_logout: "/login?logout=1&tid=...&key=...",
  page_edit_profile: "/profile?mode=editprofile",
  page_search: "",            // Vide si non configuré
  page_chatbox: "/chatbox",
  page_donate: "/buy-credits",
  page_events: "/events",
  page_imagelist: "/images",
  page_publi: "/publi",
  notifications_page: "/profile?mode=editprofile&page_profil=notifications",
  register: "/register",

  // Fonctionnalités activées (1 = actif, 0 = inactif)
  activate_toolbar: 1,       // Toolbar Forumactif visible
  fix_toolbar: 0,            // Toolbar fixée en haut au scroll
  chatbox_activate: 0,       // Chatbox activée
  chat_level: 2,             // Niveau minimum pour accéder au chat
  darkmode_exist: 1,         // Thème sombre disponible
  discover_active: 1,        // Page "Découvrir" activée
  donate: 0,                 // Système de dons actif
  event_activate: 0,         // Module événements actif
  imagelist_active: 1,       // Galerie d'images activée
  notifications: 1,          // Système de notifications actif
  publication_activate: 0,   // Module publications actif

  // Thème mobile
  tpl_mobile: "mobi_modern", // Template mobile utilisé
  tpl_used: "awesomebb",     // Template desktop utilisé
};
```

> ⚠️ **Piège fréquent** : La propriété de connexion est `session_logged_in` (entier 1/0), **pas** `is_logged_in`. Utiliser `_userdata.session_logged_in` dans tout le code JS.

### Utilisation dans un template

**Méthode 1 — `document.write`** (simple) :

```html
<div id="mon-pseudo">
  <script>
    document.write(_userdata["username"]);
  </script>
</div>
```

**Méthode 2 — jQuery (recommandée)** :

```html
<!-- Dans le template -->
<span class="js-username"></span>
<span class="js-user_posts"></span>
<div class="js-avatar"></div>

<!-- Dans un script JS (Gestion des JS ou template) -->
<script>
  $(function () {
    $.each(_userdata, function (key, value) {
      $(".js-" + key).html(value);
    });
  });
</script>
```

**Méthode 3 — Conditionnelle** (afficher selon connexion) :

```javascript
$(function () {
  if (_userdata.session_logged_in) {
    $("#bloc-connecte").show();
    $("#bloc-deconnecte").hide();
    $(".js-username").html(_userdata.username);
  } else {
    $("#bloc-connecte").hide();
    $("#bloc-deconnecte").show();
  }
});
```

**Méthode 4 — Accès aux URLs de navigation via `_userdata`** :

```javascript
// Préférer les page_* de _userdata aux variables {U_*} dans les scripts JS dynamiques
var logoutUrl = _userdata.page_logout;
var profileUrl = _userdata.page_edit_profile;
var notifUrl   = _userdata.notifications_page;
```

---

## Variables dans les messages/topics

### Dans viewtopic_body (boucle postrow)

| Variable                  | Description                    |
| ------------------------- | ------------------------------ |
| `{postrow.POSTER_NAME}`   | Pseudo de l'auteur             |
| `{postrow.POSTER_AVATAR}` | Avatar                         |
| `{postrow.POST_DATE}`     | Date du message                |
| `{postrow.MESSAGE}`       | Contenu HTML du message        |
| `{postrow.SIGNATURE}`     | Signature de l'auteur          |
| `{postrow.POSTER_POSTS}`  | Nombre de messages de l'auteur |
| `{postrow.RANK_TITLE}`    | Titre du rang                  |
| `{postrow.RANK_IMAGE}`    | Image du rang                  |
| `{postrow.MINI_POST_VAR}` | Lien direct vers le message    |
| `{postrow.U_POSTER}`      | URL du profil de l'auteur      |
| `{postrow.U_EDIT}`        | URL d'édition du message       |
| `{postrow.U_DELETE}`      | URL de suppression             |
| `{postrow.U_QUOTE}`       | URL de citation                |

### Dans index_body (boucles catrow/forum_row)

| Variable                               | Description                      |
| -------------------------------------- | -------------------------------- |
| `{catrow.cathead.CAT_DESC}`            | Nom de la catégorie              |
| `{catrow.forum_row.FORUM_NAME}`        | Nom du forum                     |
| `{catrow.forum_row.U_VIEWFORUM}`       | URL du forum                     |
| `{catrow.forum_row.FORUM_DESC}`        | Description du forum             |
| `{catrow.forum_row.POSTS}`             | Nombre de messages dans le forum |
| `{catrow.forum_row.TOPICS}`            | Nombre de sujets dans le forum   |
| `{catrow.forum_row.LAST_POST_TIME}`    | Date du dernier message          |
| `{catrow.forum_row.LAST_POST_AUTHOR}`  | Auteur du dernier message        |
| `{catrow.forum_row.LAST_POST_SUBJECT}` | Sujet du dernier message         |

---

## Variables de pages HTML (portail, pages perso)

Dans les pages HTML du portail, les variables `{USERNAME}`, `{FORUM_NAME}` etc. fonctionnent **directement** :

```html
<!-- Dans Page d'accueil → Généralités (mode HTML) -->
<p>Bienvenue {USERNAME} ! Vous êtes sur {FORUM_NAME}.</p>
<p>Il y a {TOTAL_POSTS} messages et {TOTAL_USERS} membres.</p>
```

---

## Classes CSS dynamiques AwesomeBB

Ces classes sont ajoutées automatiquement par AwesomeBB selon l'état des éléments :

```
.cat_no_new       → catégorie sans nouveau message
.cat_new          → catégorie avec nouveau message
.forum_no_new     → sous-forum sans nouveau message
.forum_new        → sous-forum avec nouveau message
.post_new         → sujet avec nouveau message
.post_no_new      → sujet sans nouveau message
.folder_locked    → sujet verrouillé
.folder_sticky    → sujet note/épinglé
.folder_announce  → sujet annonce
.folder_global    → annonce globale
```

Exemple d'utilisation CSS :

```css
/* Mettre en valeur les catégories avec de nouveaux messages */
.cat_new {
  border-left: 4px solid var(--primary);
  background: rgba(58, 134, 255, 0.05);
}

/* Icône différente pour les sujets verrouillés */
.folder_locked .folder-icon::before {
  content: "🔒";
}
```

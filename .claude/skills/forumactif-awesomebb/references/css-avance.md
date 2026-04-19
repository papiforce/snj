# CSS Avancé — Recettes pour AwesomeBB

## Palette de couleurs globale (recommandée)

Toujours déclarer les variables en haut de la feuille de style :

```css
:root {
  /* Couleurs principales */
  --primary: #3a86ff;
  --primary-dark: #2563eb;
  --secondary: #ff006e;
  --accent: #ffbe0b;

  /* Fonds */
  --bg-main: #0f0f1a;
  --bg-card: #1a1a2e;
  --bg-hover: #2a2a3e;

  /* Texte */
  --text: #e0e0e0;
  --text-muted: #888;
  --text-link: var(--primary);

  /* Espacements */
  --sm: 8px;
  --md: 16px;
  --lg: 32px;
  --xl: 64px;

  /* Effets */
  --radius: 8px;
  --shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  --transition: 0.2s ease;
}
```

---

## Navbar custom — Recettes

### Navbar sticky avec logo + liens + bouton connexion

```css
#custom-navbar {
  position: sticky;
  top: 0;
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--lg);
  height: 64px;
  background: var(--bg-card);
  box-shadow: var(--shadow);
  border-bottom: 2px solid var(--primary);
}

#custom-navbar .nav-logo img {
  height: 40px;
  width: auto;
}

#custom-navbar .nav-links {
  display: flex;
  list-style: none;
  gap: var(--md);
  margin: 0;
  padding: 0;
}

#custom-navbar .nav-links a {
  color: var(--text);
  text-decoration: none;
  font-weight: 500;
  padding: 6px 14px;
  border-radius: var(--radius);
  transition:
    background var(--transition),
    color var(--transition);
}

#custom-navbar .nav-links a:hover,
#custom-navbar .nav-links a.active {
  background: var(--primary);
  color: #fff;
}

/* Bouton CTA (connexion/inscription) */
#custom-navbar .nav-cta {
  background: var(--primary);
  color: #fff !important;
  border-radius: 999px;
  padding: 8px 20px !important;
}

/* Responsive hamburger */
#hamburger {
  display: none;
  background: none;
  border: none;
  color: var(--text);
  font-size: 28px;
  cursor: pointer;
}

@media (max-width: 768px) {
  #custom-navbar .nav-links {
    display: none;
    position: absolute;
    top: 64px;
    left: 0;
    right: 0;
    background: var(--bg-card);
    flex-direction: column;
    padding: var(--md);
    gap: var(--sm);
    box-shadow: var(--shadow);
  }
  #custom-navbar .nav-links.open {
    display: flex;
  }
  #hamburger {
    display: block;
  }
}
```

### Navbar avec sous-menus dropdown

```css
#custom-navbar .nav-links li {
  position: relative;
}

#custom-navbar .dropdown {
  display: none;
  position: absolute;
  top: 100%;
  left: 0;
  background: var(--bg-card);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  min-width: 180px;
  z-index: 10000;
  flex-direction: column;
  padding: var(--sm);
}

#custom-navbar .nav-links li:hover .dropdown {
  display: flex;
}

#custom-navbar .dropdown a {
  border-radius: 4px;
  padding: 8px 12px;
  display: block;
}
```

HTML correspondant dans `overall_header` :

```html
<li>
  <a href="#">Sections ▾</a>
  <ul class="dropdown">
    <li><a href="/forum/section1">Section 1</a></li>
    <li><a href="/forum/section2">Section 2</a></li>
  </ul>
</li>
```

---

## Cartes de forums (index_body)

```css
/* Conteneur général */
#forum-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: var(--md);
  padding: var(--lg);
}

/* Carte de forum */
.forum-card {
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: var(--md);
  box-shadow: var(--shadow);
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition:
    transform var(--transition),
    box-shadow var(--transition);
}

.forum-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
}

/* Statut nouveau message */
.forum-card.forum_new {
  border-left: 3px solid var(--primary);
}

.forum-card.forum_no_new {
  border-left: 3px solid transparent;
}
```

---

## Profil membre — Style carte

```css
#profile-card {
  display: flex;
  gap: var(--lg);
  background: var(--bg-card);
  border-radius: var(--radius);
  padding: var(--lg);
  box-shadow: var(--shadow);
}

#profile-card .avatar img {
  width: 120px;
  height: 120px;
  object-fit: cover;
  border-radius: 50%;
  border: 4px solid var(--primary);
}

#profile-card .info h1 {
  color: var(--text);
  font-size: 1.8rem;
  margin: 0 0 var(--sm) 0;
}

#profile-card .stats {
  display: flex;
  gap: var(--md);
  flex-wrap: wrap;
  margin-top: var(--md);
}

#profile-card .stat {
  background: var(--bg-main);
  padding: var(--sm) var(--md);
  border-radius: var(--radius);
  text-align: center;
}

#profile-card .stat .value {
  font-size: 1.5rem;
  font-weight: bold;
  color: var(--primary);
}
```

---

## Cacher/Modifier des éléments Forumactif natifs

```css
/* Cacher la toolbar Forumactif du haut */
#page-top {
  display: none !important;
}

/* Cacher le footer Forumactif */
#footer-top {
  display: none !important;
}

/* Cacher le logo Forumactif en bas */
.copyright {
  display: none !important;
}

/* Cacher la barre de search native */
#searchbar {
  display: none !important;
}

/* Cacher les publicités (si forum premium) */
.ad-wrapper {
  display: none !important;
}

/* Modifier la largeur du contenu principal */
#page-body .wrap {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--md);
}
```

---

## Animations et effets

```css
/* Apparition en fondu */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.forum-card {
  animation: fadeIn 0.3s ease forwards;
}

/* Effet de chargement skeleton */
@keyframes shimmer {
  0% {
    background-position: -200% 0;
  }
  100% {
    background-position: 200% 0;
  }
}

.skeleton {
  background: linear-gradient(
    90deg,
    var(--bg-card) 25%,
    var(--bg-hover) 50%,
    var(--bg-card) 75%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
  border-radius: var(--radius);
}
```

---

## Responsive — Breakpoints recommandés

```css
/* Desktop large */
@media (min-width: 1280px) {
}

/* Desktop standard */
@media (max-width: 1279px) {
}

/* Tablette */
@media (max-width: 1024px) {
  #forum-list {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Mobile large */
@media (max-width: 768px) {
  #forum-list {
    grid-template-columns: 1fr;
  }
  #profile-card {
    flex-direction: column;
  }
}

/* Mobile petit */
@media (max-width: 480px) {
  :root {
    --md: 12px;
    --lg: 20px;
  }
}
```

---

## Thème sombre / clair (toggle)

```css
/* Thème clair */
body.theme-light {
  --bg-main: #f5f5f5;
  --bg-card: #ffffff;
  --text: #1a1a1a;
  --text-muted: #555;
}

/* Bouton toggle dans le template */
```

```javascript
// JS pour le toggle
document.getElementById("theme-toggle").addEventListener("click", function () {
  document.body.classList.toggle("theme-light");
  localStorage.setItem(
    "theme",
    document.body.classList.contains("theme-light") ? "light" : "dark",
  );
});

// Restaurer le thème au chargement
if (localStorage.getItem("theme") === "light") {
  document.body.classList.add("theme-light");
}
```

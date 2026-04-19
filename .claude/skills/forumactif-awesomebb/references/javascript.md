# JavaScript — Intégration AwesomeBB

## Environnement JS sur Forumactif

- **jQuery** est disponible nativement sur tous les forums Forumactif
- Les scripts sont ajoutés via **Affichage → Gestion des codes JavaScript**
- Depuis déc. 2025 : éditeur JS intégré, désactivation en 1 clic, bouton télécharger
- `_userdata` est un objet global disponible sur toutes les pages AwesomeBB
- Attention : les MAJ Forumactif peuvent modifier des sélecteurs CSS/JS (ex. pagination fév. 2026)

---

## Script 1 — Afficher les données utilisateur partout

```javascript
// À placer dans Affichage → Gestion des codes JavaScript
// Appliqué à toutes les pages

$(function () {
  // Remplir tous les éléments avec la classe .js-[propriété]
  $.each(_userdata, function (key, value) {
    $(".js-" + key).html(value);
  });

  // Gérer l'état connecté/déconnecté
  if (_userdata.session_logged_in) {
    $(".show-logged-in").show();
    $(".show-logged-out").hide();
  } else {
    $(".show-logged-in").hide();
    $(".show-logged-out").show();
  }
});
```

Usage dans le template `overall_header` :

```html
<!-- Afficher le pseudo -->
<span class="js-username"></span>

<!-- Afficher l'avatar -->
<div class="js-avatar"></div>

<!-- Bloc visible seulement si connecté -->
<div class="show-logged-in" style="display:none;">
  Bonjour <span class="js-username"></span> !
  <a href="{U_PRIVATEMAIL}">MP (<span class="js-user_nb_privmsg"></span>)</a>
</div>

<!-- Bloc visible si non connecté -->
<div class="show-logged-out" style="display:none;">
  <a href="{U_LOGIN_LOGOUT}">Se connecter</a> |
  <a href="{U_REGISTER}">S'inscrire</a>
</div>
```

---

## Script 2 — Navbar hamburger responsive

```javascript
// Toggle menu mobile
function toggleMenu() {
  var navLinks = document.querySelector("#custom-navbar .nav-links");
  navLinks.classList.toggle("open");
}

// Fermer le menu en cliquant ailleurs
document.addEventListener("click", function (e) {
  if (!document.querySelector("#custom-navbar").contains(e.target)) {
    var navLinks = document.querySelector("#custom-navbar .nav-links");
    if (navLinks) navLinks.classList.remove("open");
  }
});
```

---

## Script 3 — Marquer la page active dans la navbar

```javascript
$(function () {
  var currentUrl = window.location.href;
  $("#custom-navbar .nav-links a").each(function () {
    if (currentUrl.indexOf($(this).attr("href")) !== -1) {
      $(this).addClass("active");
    }
  });
});
```

---

## Script 4 — Notification de nouveaux MP

```javascript
$(function () {
  var pmCount = parseInt(_userdata.user_nb_privmsg || 0);
  if (pmCount > 0) {
    // Ajouter un badge rouge
    $("#pm-icon").append('<span class="pm-badge">' + pmCount + "</span>");
  }
});
```

CSS pour le badge :

```css
#pm-icon {
  position: relative;
  display: inline-block;
}
.pm-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  background: #ff006e;
  color: #fff;
  font-size: 10px;
  font-weight: bold;
  border-radius: 999px;
  padding: 2px 6px;
  min-width: 18px;
  text-align: center;
}
```

---

## Script 5 — Retour en haut de page

```javascript
$(function () {
  // Créer le bouton
  $("body").append(
    '<button id="back-to-top" title="Retour en haut">↑</button>',
  );

  // Afficher/masquer selon le scroll
  $(window).scroll(function () {
    if ($(this).scrollTop() > 300) {
      $("#back-to-top").fadeIn();
    } else {
      $("#back-to-top").fadeOut();
    }
  });

  // Action au clic
  $("#back-to-top").click(function () {
    $("html, body").animate({ scrollTop: 0 }, 400);
  });
});
```

CSS :

```css
#back-to-top {
  display: none;
  position: fixed;
  bottom: 30px;
  right: 30px;
  background: var(--primary);
  color: #fff;
  border: none;
  border-radius: 50%;
  width: 44px;
  height: 44px;
  font-size: 20px;
  cursor: pointer;
  z-index: 9999;
  box-shadow: var(--shadow);
  transition: background 0.2s;
}
#back-to-top:hover {
  background: var(--primary-dark);
}
```

---

## Script 6 — Toggle thème sombre/clair

```javascript
$(function () {
  // Restaurer la préférence sauvegardée
  if (localStorage.getItem("fa-theme") === "light") {
    $("body").addClass("theme-light");
    $("#theme-toggle").text("🌙");
  }

  // Basculer au clic
  $("#theme-toggle").click(function () {
    $("body").toggleClass("theme-light");
    var isLight = $("body").hasClass("theme-light");
    localStorage.setItem("fa-theme", isLight ? "light" : "dark");
    $(this).text(isLight ? "🌙" : "☀️");
  });
});
```

---

## Script 7 — Confirmer la déconnexion

```javascript
$(function () {
  // Intercepter le clic sur le lien déconnexion
  $('a[href*="mode=logout"]').click(function (e) {
    if (!confirm("Voulez-vous vraiment vous déconnecter ?")) {
      e.preventDefault();
    }
  });
});
```

---

## Script 8 — Pagination AwesomeBB (sélecteur correct depuis fév. 2026)

> ⚠️ La pagination a été modifiée en fév. 2026. Utiliser ce sélecteur :

```javascript
// Sélecteur correct pour AwesomeBB
var paginationSelector = ".pagination:not(strong)";

$(paginationSelector).each(function () {
  // Votre traitement de pagination
});
```

---

## Bonnes pratiques JS sur Forumactif

1. **Toujours envelopper dans `$(function(){ })` ou `$(document).ready()`** pour attendre le chargement du DOM.

2. **Éviter les conflits** : Chaque script ajouté dans la gestion des JS est chargé sur toutes les pages. Tester soigneusement.

3. **Vérifier après une mise à jour** : Forumactif peut changer des sélecteurs CSS/JS sans préavis. Suivre [les annonces](https://forum.forumactif.com/f1-annonces-mises-a-jour).

4. **Désactiver temporairement** : Depuis déc. 2025, vous pouvez désactiver un script en 1 clic dans la gestion des JS sans le supprimer.

5. **Utiliser `_userdata` avec précaution** : L'objet est vide ou incomplet pour les visiteurs non connectés. Toujours vérifier `_userdata.is_logged_in` avant d'accéder aux propriétés.

6. **Cibler précisément** : Préférer `#id` plutôt que `.class` pour éviter de modifier des éléments inattendus.

7. **Tester sur mobile** : AwesomeBB est responsive. Vérifier que vos scripts n'endommagent pas l'affichage mobile.

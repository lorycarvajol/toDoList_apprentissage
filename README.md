---

## Todo List Simple

Ce projet est une petite application JavaScript qui permet de gérer une liste de tâches. Elle illustre les concepts suivants :

* Sélection d’éléments dans le DOM avec `getElementById`
* Gestion d’événements via `addEventListener`
* Manipulation de chaînes de caractères avec `trim`
* Stockage temporaire des tâches dans un tableau (`tasks`)
* Rendu dynamique de la liste avec la fonction `renderTasks`

---

### Structure du projet

* `index.html`
  Contient le formulaire (champ texte + bouton) et la zone d’affichage (`<ul>`) des tâches.
* `<style>`
  Quelques règles CSS pour un rendu simple et agréable.
* `<script>`
  Logique JavaScript détaillée ci-dessous.

---

## Explications détaillées

### 1. `document.getElementById(id)`

* **But** : Récupérer une référence à un élément du DOM dont l’attribut `id` vaut la valeur fournie.
* **Retour** : Un objet `HTMLElement` (ou `null` si aucun élément trouvé).

```js
const taskInput  = document.getElementById("taskInput");
const addTaskBtn = document.getElementById("addTaskBtn");
const taskList   = document.getElementById("taskList");
```

> Ici, on récupère :
>
> * `taskInput` → l’`<input>` pour saisir la nouvelle tâche
> * `addTaskBtn` → le `<button>` « Ajouter »
> * `taskList` → le `<ul>` où seront injectées les tâches

---

### 2. `element.addEventListener(event, callback)`

* **But** : Attacher un gestionnaire d’événement à un élément du DOM.
* **Paramètres** :

  * `event` (string) : le nom de l’événement (ex. `"click"`, `"input"`, `"keyup"`, …)
  * `callback` (function) : la fonction à exécuter lorsque l’événement se produit

```js
addTaskBtn.addEventListener("click", () => {
  // code exécuté au clic sur le bouton
});
```

> À chaque clic sur « Ajouter », on déclenche la fonction fournie.

---

### 3. `String.prototype.trim()`

* **But** : Supprimer les espaces blancs en début et fin de chaîne.
* **Retour** : Une nouvelle chaîne sans ces espaces.

```js
const newTask = taskInput.value.trim();
```

> Utile pour éviter que l’utilisateur envoie uniquement des espaces.

---

### 4. Le tableau `tasks`

```js
let tasks = [];
```

* **Rôle** : Stocker en mémoire vive (dans la session) la liste des tâches sous forme de chaînes.
* **Opérations principales** :

  * **Ajouter** : `tasks.push(nouvelleTâche)`
  * **Supprimer** : `tasks.splice(index, 1)`

---

### 5. La fonction `renderTasks()`

```js
function renderTasks() {
  // 1. On vide le contenu HTML actuel
  taskList.innerHTML = "";

  // 2. Pour chaque tâche stockée...
  tasks.forEach((task, index) => {
    //  - on crée un <li>
    const li = document.createElement("li");
    li.textContent = task;

    //  - on ajoute un bouton "Supprimer"
    const deleteBtn = document.createElement("button");
    deleteBtn.textContent = "Supprimer";

    //  - on gère le clic sur "Supprimer"
    deleteBtn.addEventListener("click", () => {
      tasks.splice(index, 1);
      renderTasks(); // on ré-affiche la liste
    });

    //  - on assemble les éléments
    li.appendChild(deleteBtn);
    taskList.appendChild(li);
  });
}
```

#### Étapes détaillées

1. **Vider l’affichage**

   ```js
   taskList.innerHTML = "";
   ```

   Cela évite les doublons à chaque mise à jour.

2. **Boucle `forEach`**

   * Parcours chaque élément du tableau `tasks`.
   * `task` = contenu (string) ; `index` = position dans le tableau.

3. **Création d’éléments dynamiques**

   * `document.createElement("li")` → crée un `<li>`.
   * `li.textContent = task` → insère le texte de la tâche.
   * Même approche pour le bouton de suppression.

4. **Réaction au clic sur Supprimer**

   * `tasks.splice(index, 1)` → retire 1 élément à la position `index`.
   * On rappelle `renderTasks()` pour que l’interface reflète le changement.

---

## Workflow global

1. L’utilisateur saisit une tâche et clique sur **Ajouter**.
2. On récupère la chaîne, on la « trim », on l’`push` dans `tasks`.
3. On efface le champ de saisie.
4. On appelle `renderTasks()` :

   * on reconstruit la liste à partir de `tasks`.
   * chaque `<li>` comporte son bouton **Supprimer**.
5. Si l’utilisateur clique sur un **Supprimer**, on retire la tâche puis on ré-affiche la liste.

---

## Pour aller plus loin

* **Stockage persistant** : utilisez `localStorage` pour conserver la liste entre deux visites.
* **Édition de tâches** : ajoutez un bouton « Modifier » et affichez un champ de saisie inline.
* **Filtrage/Recherche** : ajoutez un champ permettant de filtrer la liste existante.

---



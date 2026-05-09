# La gestion d'erreurs en Rust

## Option\<T\>

`Option<T>` représente une valeur qui peut être présente (`Some(T)`) ou absente (`None`).

### Création

```rust
let some_val: Option<i32> = Some(42);
let none_val: Option<i32> = None;
```

### Match / if let

```rust
match some_val {
    Some(v) => println!("{}", v),
    None => println!("rien"),
}

if let Some(v) = some_val {
    println!("{}", v);
}
```

### Méthodes principales

| Méthode              | Description                                 |
| -------------------- | ------------------------------------------- |
| `unwrap()`           | Panique si `None`                           |
| `expect("msg")`      | Panique avec message si `None`              |
| `unwrap_or(default)` | Valeur par défaut si `None`                 |
| `unwrap_or_else(fn)` | Appelle une fonction si `None`              |
| `is_some()`          | `true` si `Some`                            |
| `is_none()`          | `true` si `None`                            |
| `ok_or(err)`         | Convertit en `Result` (`None` → `Err(err)`) |

### Combinators

```rust
// map — transforme la valeur intérieure si Some
let a = Some(5).map(|x| x * 2);            // Some(10)
let b: Option<i32> = None.map(|x| x * 2);  // None

// and_then — chaîne d'options (flat_map)
let c = Some(5).and_then(|x| Some(x * 2)); // Some(10)

// filter — garde Some si condition vraie
let d = Some(42).filter(|&x| x > 10);      // Some(42)

// or / or_else — fallback si None
let e = None.or(Some(0));                   // Some(0)

// get_or_insert — insère si None, retourne ref
let mut f = None;
f.get_or_insert(99);                        // maintenant Some(99)

// take — remplace par None, retourne l'ancienne valeur
let mut g = Some(5);
let h = g.take();                           // h = Some(5), g = None

// replace — remplace par une nouvelle valeur
let mut i = Some(3);
let j = i.replace(42);                      // j = Some(3), i = Some(42)
```


## Result\<T, E\>

`Result<T, E>` représente une opération qui peut réussir (`Ok(T)`) ou échouer (`Err(E)`).

### Créationtheora59@theora59-B550M-HDV:~/Projets/Blog$ mkdocs serve
ERROR   -  Config value 'theme': Unrecognised theme name: 'material'. The
           available installed themes are: mkdocs, readthedocs
ERROR   -  Config value 'markdown_extensions': Failed to load extension
           'pymdownx.highlight'.
           ModuleNotFoundError: No module named 'pymdownx'

Aborted with 2 configuration errors!
theora59@theora59-B550M-HDV:~/Projets/Blog$ mkdocs serve 


```rust
let ok: Result<i32, &str> = Ok(42);
let err: Result<i32, &str> = Err("échec");
```

### Match / if let

```rust
match ok {
    Ok(v) => println!("{}", v),
    Err(e) => eprintln!("{}", e),
}

if let Ok(v) = ok {
    println!("{}", v);
}
```

### Méthodes principales

| Méthode                | Description                                |
| ---------------------- | ------------------------------------------ |
| `unwrap()`             | Panique si `Err`                           |
| `expect("msg")`        | Panique avec message si `Err`              |
| `unwrap_or(default)`   | Valeur par défaut si `Err`                 |
| `unwrap_or_else(fn)`   | Appelle une fonction si `Err`              |
| `is_ok()` / `is_err()` | Teste le variant                           |
| `ok()`                 | Convertit en `Option` (`Err` → `None`)     |
| `err()`                | Convertit en `Option<Err>` (`Ok` → `None`) |

### Combinators

```rust
// map — transforme Ok
let a: Result<i32, &str> = Ok(5).map(|x| x * 2);   // Ok(10)

// map_err — transforme Err
let b: Result<i32, &str> = Err("x").map_err(|e| format!("err: {}", e));

// and_then — chaîne (flat_map)
let c = Ok(5).and_then(|x| Ok(x * 2));              // Ok(10)

// or_else — fallback si Err
let d = Err("echec").or_else(|_| Ok(0));            // Ok(0)

// unwrap_or_else — défaut via closure
let e = Err("echec").unwrap_or_else(|_| 42);         // 42

// map_or — valeur par défaut + transformation
let f = Ok(5).map_or(0, |x| x * 2);                 // 10

// map_or_else — deux closures
let g = Err("echec").map_or_else(|e| e.len(), |x: i32| x as usize);
```

---

## L'opérateur `?`

Propagation d'erreur : retourne `Err` immédiatement si `Result` est `Err`, ou `None` si `Option` est `None`.

```rust
use std::fs::File;
use std::io::Read;

fn read_file(path: &str) -> Result<String, std::io::Error> {
    let mut f = File::open(path)?;      // si Err, return Err immédiatement
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}

fn get_first(items: &[i32]) -> Option<&i32> {
    let first = items.first()?;         // si None, return None
    Some(first)
}
```

**Attention :** `?` dans une `main()` nécessite un type de retour `Result`.

```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    let s = read_file("test.txt")?;
    println!("{}", s);
    Ok(())
}
```

---

## Créer ses propres erreurs

### Erreur simple (type énuméré + Display)

```rust
use std::fmt;

#[derive(Debug)]
enum MonErreur {
    FichierIntrouvable,
    PermissionRefusee,
    ValeurInvalide(String),
}

impl fmt::Display for MonErreur {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            MonErreur::FichierIntrouvable => write!(f, "fichier introuvable"),
            MonErreur::PermissionRefusee => write!(f, "permission refusée"),
            MonErreur::ValeurInvalide(v) => write!(f, "valeur invalide: {}", v),
        }
    }
}

impl std::error::Error for MonErreur {}  // trait bound
```

### Erreur avec cause (wrapping d'erreurs)

```rust
use std::fmt;
use std::error::Error;

#[derive(Debug)]
struct MonErreur {
    message: String,
    source: Option<Box<dyn Error>>,
}

impl fmt::Display for MonErreur {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{}", self.message)
    }
}

impl Error for MonErreur {
    fn source(&self) -> Option<&(dyn Error + 'static)> {
        self.source.as_ref().map(|e| e.as_ref() as &(dyn Error + 'static))
    }
}
```

### `From` pour conversion automatique

```rust
use std::io;

impl From<io::Error> for MonErreur {
    fn from(err: io::Error) -> Self {
        MonErreur {
            message: format!("erreur IO: {}", err),
            source: Some(Box::new(err)),
        }
    }
}
```

Permet d'utiliser `?` avec `io::Error` dans des fonctions retournant `MonErreur` :

```rust
fn lire(path: &str) -> Result<String, MonErreur> {
    let s = std::fs::read_to_string(path)?;  // io::Error → MonErreur auto
    Ok(s)
}
```

### `thiserror` (crate — recommandé)

```rust
use thiserror::Error;

#[derive(Error, Debug)]
enum MonErreur {
    #[error("fichier introuvable")]
    FichierIntrouvable,

    #[error("permission refusée: {0}")]
    PermissionRefusée(String),

    #[error("erreur IO: {0}")]
    Io(#[from] std::io::Error),  // From + Display auto
}
```

## Bonnes pratiques

| Situation                         | Approche                           |
| --------------------------------- | ---------------------------------- |
| Prototypage / scripts             | `unwrap()`, `expect()`             |
| Propagation simple                | `?`                                |
| App / CLI                         | `anyhow::Result`                   |
| Bibliothèque / production         | Énumération d'erreur personnalisée |
| Erreurs complexes en bibliothèque | `thiserror`                        |
| Conversion Option → Result        | `ok_or()` / `ok_or_else()`         |

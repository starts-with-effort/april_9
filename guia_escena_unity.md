
# ✈️ Guía para Crear una Escena Interactiva en Unity 3D

En esta actividad construiremos una escena en Unity en la que un **avión vuela hacia un edificio**, colisiona con él, y se genera una **explosión con sonido**. Esta guía paso a paso está pensada para estudiantes que se están iniciando en el desarrollo de videojuegos con Unity.

---

## 🧱 Paso 1: Buscar Assets en la Tienda de Unity

1. Abre Unity y dirígete al menú superior:  
   `Window > Asset Store (o abrir desde navegador)`
   ![Imagen Unity Assets Store](https://drive.google.com/uc?id=1hDez1Hw1wtYSUvjCid_VpKDmrwad7S19)
2. Busca y descarga los siguientes elementos:
   - ✈️ Un modelo de **avión**
   - 🏢 Un **edificio** o torre
   - 💥 Un **efecto de explosión** (con partículas y sonido si es posible)
   ![Imagen Unity Assets Store](https://drive.google.com/uc?id=1lqMXWH6CwPAQB8nzItA3UyAd_AojO7FJ)
3. Una vez descargados, haz clic en **Importar** para agregarlos a tu proyecto.

---

## 📦 Paso 2: Añadir los Assets a tu Proyecto

- Asegúrate de que todos los assets estén visibles en la carpeta `Assets/`.
- Si es necesario, crea una nueva carpeta llamada `Models` o `Prefabs` para mantener todo organizado.
![Imagen Unity Assets Store](https://drive.google.com/uc?id=1GWu6h4dFGatGtuDqTRQLi1tLDGrD2XUI)

---

## ✈️ Paso 3: Importar el Avión a la Escena

1. Arrastra el modelo del **avión** desde el panel de proyecto a la jerarquía de la escena.
2. Asegúrate de que esté bien orientado (eje **Z hacia adelante**).
![Imagen Unity Assets Store](https://drive.google.com/uc?id=1Jt6YmElfxLAfmWwF6-pr5S-unH_DOqYg)

---

## 🧱 Paso 4: Añadir un Box Collider al Avión

1. Selecciona el avión en la jerarquía.
2. En el `Inspector`, haz clic en **Add Component**.
3. Busca y añade un **Box Collider**.
![Imagen Unity Assets Store](https://drive.google.com/uc?id=1s0YVmQXC1-cgWYw5CKIMDtEsJIypGn88)

---

## 🏢 Paso 5: Importar el Edificio a la Escena

- Arrastra el modelo del edificio a la escena y colócalo delante del avión (a una buena distancia para que el avión tenga tiempo de volar hacia él).
![Imagen Unity Assets Store](https://drive.google.com/uc?id=1xw9To8htRKlXj1ToJObI2VAlFjcwGvVb)

---

## 🧱 Paso 6: Añadir un Box Collider al Edificio

- Igual que con el avión, selecciona el edificio y añade un **Box Collider** desde el `Inspector`.
![Imagen Unity Assets Store](https://drive.google.com/uc?id=1K_BNm-Lo6cRrhmV2mTbV8QKnmlKpcJb1)

---

## 🧲 Paso 7: Añadir Rigidbody al Edificio y Fijarlo

1. Selecciona el edificio.
2. Haz clic en **Add Component > Rigidbody**.
3. En el `Inspector`, marca la opción ✅ `Is Kinematic` para que no se mueva al colisionar.
![Imagen Unity Assets Store](https://drive.google.com/uc?id=15QfuirkB938egdzVfwYD4GbBhj7zSwaS)

---

## 💨 Paso 8: Programar el Movimiento del Avión

### 1. Crear el script

- Crea un nuevo script en `Assets/Scripts/AvionMovimiento.cs` con el siguiente contenido:

```csharp
using UnityEngine;

public class AvionMovimiento : MonoBehaviour
{
    public float velocidad = 5f;

    void Update()
    {
        transform.Translate(Vector3.forward * velocidad * Time.deltaTime);
    }
}
```

### 2. Asignar el script

- Asigna el script al avión desde el `Inspector`.
![Imagen Unity Assets Store](https://drive.google.com/uc?id=1SWGHeAbuUMwixkyAG9lilciNnqXyMc8V)

---

## 🎥 Paso 9: Hacer que la Cámara Siga al Avión

### Opción Simple (Script)

1. Crea un nuevo script llamado `SeguirCamara.cs`:

```csharp
using UnityEngine;

public class SeguirCamara : MonoBehaviour
{
    public Transform objetivo;
    public Vector3 offset = new Vector3(0, 5, -10);

    void LateUpdate()
    {
        if (objetivo != null)
            transform.position = objetivo.position + offset;
    }
}
```

2. Asigna este script a la cámara.
3. En el campo `Objetivo`, arrastra el avión.
4. Ajusta el `Offset` si quieres un ángulo diferente.

![Imagen Unity Assets Store](https://drive.google.com/uc?id=1XTEQvyK93So8xgIcaYOHvs4U7-G7pBJd)

---

## 💥 Paso 10: Programar la Explosión al Colisionar

### 1. Crear un punto de explosión

1. Haz clic derecho sobre el avión en la jerarquía:  
   `Create Empty` → llámalo `PuntoExplosion`
2. Colócalo en la **punta del avión** (`Position Z` adelantado)

### 2. Crear el script `ColisionExplosion.cs`:

```csharp
using UnityEngine;

public class ColisionExplosion : MonoBehaviour
{
    public GameObject explosionPrefab;
    public Transform puntoExplosion;
    public AudioClip sonidoExplosion;

    private AudioSource audioSource;

    void Start()
    {
        audioSource = gameObject.AddComponent<AudioSource>();
    }

    void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.tag == "Torre")
        {
            Instantiate(explosionPrefab, puntoExplosion.position, Quaternion.identity);
            audioSource.PlayOneShot(sonidoExplosion);

            Destroy(gameObject);
            Destroy(collision.gameObject);
        }
    }
}
```

![Imagen Unity Assets Store](https://drive.google.com/uc?id=17geN4EV7IqeDQQGqR5SoTSKFHp5DIaKh)

### 3. Asignar elementos

- Asigna este script al avión.
- Asigna:
  - `ExplosionPrefab` → Prefab de la explosión
  - `PuntoExplosion` → El Empty creado
  - `SonidoExplosion` → Clip de sonido
- Asegúrate de que el edificio tenga el **tag "Torre"** (crea el tag si no existe).

---

## ▶️ Paso 11: Ejecutar la Escena

- Presiona el botón **Play** en la parte superior de Unity.
- Verifica que el avión:
  - Se mueva hacia adelante
  - Sea seguido por la cámara
  - Colisione con la torre
  - Genere explosión y sonido

---

## ✅ ¡Listo!

¡Felicidades! Has creado una mini escena interactiva con movimiento, colisión, partículas y sonido.

---


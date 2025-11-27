# GDV-M2
GDV game concept 

 

In mijn game ben je in een oerwoud en is je goal om alle apen uit de bomen te gooien met stenen, je krijgt een bepaald aantal stenen en als die op zijn vallen de resterende apen je aan en is het game over. 

De main machenic van de game is dat de stenen 1 keer kunnen stuiteren, zo moet je dus inschatten om waar de steen naartoe vliegt en of het de aap zal raken die achter een tak/boom verstopt zit. 

# foto

<img width="640" height="415" alt="Screenshot 2025-11-24 121143" src="https://github.com/user-attachments/assets/f63875ae-92af-4f34-9c33-0ce16b9eefc3" />


# kort filmpje van de basis opdracht

![adcy58](https://github.com/user-attachments/assets/4c87bb91-8783-49fd-acec-638381b77af8)

```Csharp
using UnityEngine;

public class BallShooter2D : MonoBehaviour
{
    [Header("Ball Settings")]
    public GameObject ballPrefab;� �// Prefab of the ball to shoot
� � public Transform shootPoint;� � // Position from where the ball will be shot
� � public float shootForce = 50000f; // Force applied to the ball

� � [Header("Ball Lifetime")]
    public float ballLifetime = 5f; // Time before the ball is destroyed

� � void Update()
    {
� � � � // Shoot when spacebar is pressed
� � � � if (Input.GetKeyDown(KeyCode.Space))
        {
            ShootBall();
        }
    }

    void ShootBall()
    {
        if (ballPrefab == null || shootPoint == null)
        {
            Debug.LogWarning("Ball prefab or shoot point not assigned!");
            return;
        }

� � � � // 1. Instantiate the ball (Position is nu 2D Vector3)
� � � � GameObject ball = Instantiate(ballPrefab, shootPoint.position, shootPoint.rotation);

� � � � // 2. GEBRUIK Rigidbody2D
� � � � Rigidbody2D rb = ball.GetComponent<Rigidbody2D>();

� � � � // Zorg ervoor dat de 2D Rigidbody is gevonden
� � � � if (rb == null)
        {
            Debug.LogError("Ball Prefab must have a Rigidbody2D component!");
� � � � � � // We kunnen hier AddComponent proberen, maar het is beter om het op de Prefab te hebben.
� � � � � � return;
        }

        // 3. Gebruik Rigidbody2D.velocity in plaats van 3D AddForce met Transform.forward
        // We gebruiken 'up' voor 2D projectielen tenzij je wilt schieten in de X-richting.
        // Als je wilt schieten in de richting waar 'shootPoint' naar kijkt (forward), gebruik dan de 2D-versie van forward/up.

        // Optie A: Schiet naar boven in de lokale 2D ruimte (gebruikelijk voor 'shooters')
        // Schiet naar boven (de Y-as)
        Vector2 shootDirection = Vector2.up;

        // Optie B: Schiet naar voren in de 3D-ruimte geprojecteerd naar 2D
        // Vector2 shootDirection = new Vector2(shootPoint.forward.x, shootPoint.forward.y);

        rb.AddForce(shootDirection * shootForce);

� � � � // Destroy the ball after a set time
� � � � Destroy(ball, ballLifetime);
    }
}

```

```Csharp
using UnityEngine;

public class hit : MonoBehaviour
{
    void OnCollisionEnter2D(Collision2D collision)
    {
        Debug.Log("Hit!");
    }
}
```

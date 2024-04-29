# SME-Assignment-Outscal (Space-Invaders)

Task 1 :
I successfully setup the Visual Studio Community 2022 and SFML Headers and library as instructed in the manual. copies the header and lib folders from SFML zip and paste it in sfml folder. Then I ran the .sln file in the visual studio and as instructed it gave me 2 delibrate errors.
it took me around 20mints
- Firstly, I was supposed to fork and clone the repository to my local device.
- Please refer: https://github.com/RanjithT17/Outscal-SME-Assignment.git
Then the next step was to add the SFML 2.6.1 files to my project following the setup guide.
####################################################################################

Task 2 :
- Executed in approx 40 minutes.
- I had to find a deliberate bug in the code, that was preventing the game to run. 
- The bug was in space invasion -> header -> Player -> PlayerController.h
-To solve the bug we just had to define the two class **"class PlayerModel; " and "class PlayerView;"**
-I used forward declaration to solve this part
-This was done because the file was accesssing these two classes without definiing them in the file.
####################################################################################

Task 3:
- Executed in nearly 1 hour.
- spent approximately 2hour to try to build the game from scratch using Unity.
- I added the processBuletfire() function to the PLayerController.cpp
- The Event Left mouse Click demands a bullet fire.
- A function processBulletFire() was defined under PlayerController.h file and Playercontroller.cpp
- This functions calls the spawnParticleSystem() function in the ParticleSystem. 
- This initialises the Bullet at the starting point (that is the current location of the player).
- A Bullet fire sound effect was called from the Global -> ServiceLocator.cpp.

- Further more I spent some making these scripts:
  1) Player.cs:


using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;
public class Player : MonoBehaviour
{
    [SerializeField] float moveSpeed=5f;
    Vector2 rawInput;
    Vector2 minBounds;
    Vector2 maxBounds; 
    Shooter shooter;

    void Awake()
    {
        shooter=GetComponent<Shooter>(); 
    }

    void Start() {

        InitBounds();
        
    }
    void Update()
    {
        Move();
    }

    void InitBounds()
    {
        Camera mainCamera=Camera.main;
        minBounds=mainCamera.ViewportToWorldPoint(new Vector2(0,0));
        maxBounds=mainCamera.ViewportToWorldPoint(new Vector2(1,1));
    }

    void Move()
    {
        Vector2 delta = rawInput * moveSpeed * Time.deltaTime;
        Vector2 newPos= new Vector2();
        newPos.x = Mathf.Clamp(transform.position.x + delta.x,minBounds.x,maxBounds.x);
        newPos.y = Mathf.Clamp(transform.position.y + delta.y,minBounds.y,maxBounds.y);
        transform.position = newPos;
    }

    void OnMove(InputValue value)
    {
        rawInput=value.Get<Vector2>();
        
    }

    void onFire(InputValue value)
    {
        if(shooter!=null)
        {
            shooter.isFiring=value.isPressed;
        }
    }

}

2) Shooter.cs :

   
using System.Collections;
using System.Collections.Generic;
using Unity.Mathematics;
using UnityEngine;

public class Shooter : MonoBehaviour
{
    [SerializeField] GameObject projectilePrefab;
    [SerializeField] float projectileSpeed=10f;
    [SerializeField] float projectileLifetime=5f;
    [SerializeField] float firingRate=0.2f;

    public bool isFiring;
    Coroutine firingCoroutine;

    void Update()
    {
        Fire();
    }
    void Fire()
    {
        if(isFiring)
        {
        firingCoroutine=StartCoroutine(FireContinuosly());
        }
        else{
            StopCoroutine(firingCoroutine);
        }
    }

    IEnumerator FireContinuosly()
    {
        while(true)
        {
            GameObject instance = Instantiate(projectilePrefab,transform.position,quaternion.identity);
            Destroy(instance,projectileLifetime);
            yield return new WaitForSeconds(firingRate);
        }
    }
}

I used coroutines to instatiate bullets/lasers 
####################################################################################

Task 4:

I was so challenging and interesting Task and  I am still working on this part to make it better in future :
Thank you....


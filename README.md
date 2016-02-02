# Global-game-jam-robbie-the-robots


TimerScript
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public class TimerScript : MonoBehaviour {

    public float timeLeft;
    public Text TestText;

    
    void Start () {
        TestText = GetComponent<Text>();
    }
	
	void Update () {
     
    }

    void OnGUI()
    {
        GUI.Label(new Rect(50, 50, 200, 30), "");
    }
}



Game Manager

using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using UnityEngine.SceneManagement;

namespace Completed
{
    using System.Collections.Generic;        

    public class GameManager : MonoBehaviour
    {
        
        public static GameManager instance = null;              
        private BoardManager boardScript;                       

        public TimerScript testText;
        public float timeLeft;
        public PlayerController playerController;
        public bool playerHitTrap;
        public float timeRemaining;
                     

      
       
        void Awake()
        {
          
            
            
            
            
            if (instance == null)

                
                instance = this;

           
            else if (instance != this)

                
                Destroy(gameObject);

            
            DontDestroyOnLoad(gameObject);

            
            boardScript = GetComponent<BoardManager>();

            testText = GetComponent<TimerScript>();

            
            InitGame();
        }

        
        void InitGame()
        {

            testText.timeLeft = timeRemaining;
            
        }

       
        void Update()
        {
            
            timeLeft = testText.timeLeft;
            timeLeft -= Time.deltaTime;
            
            if (timeLeft < 0)
            {
                SceneManager.LoadScene("gameover");               
            }
        }
    }
}


Camera

public class CameraController : MonoBehaviour {

    public GameObject followTarget;

    private Vector3 targetPos;
    public float moveSpeed;
	
	void Start () 
	{
	
	}
	void Update () {
        targetPos = new Vector3(followTarget.transform.position.x, followTarget.transform.position.y, transform.position.z);
        transform.position = Vector3.Lerp(transform.position, targetPos, moveSpeed * Time.deltaTime);
	}
}

Fake Shrine script

using UnityEngine;
using System.Collections;

public class Fake1 : MonoBehaviour
{

    Animator ThisAnimator;
    Collider2D collider;
    void Start()
    {
        collider = GetComponent<Collider2D>();
        ThisAnimator = GetComponent<Animator>();
    }
    void OnCollisionEnter2D(Collision2D collider)
    {
        if (collider.gameObject.tag == "Player")
        {
            ThisAnimator.SetBool("usedShrine", true);
        }
    }
}

Level Manager

using UnityEngine;
using System.Collections;

public class LevelManager : MonoBehaviour
{
    public Transform mainMenu, credits;
    public void  LoadScene(string name)
        
    {
        Application.LoadLevel(name);
           

    }
    public void QuitGame(string name)
    {
        Application.Quit();
    }
    public void Credits(bool clicked)
    {

        if (clicked == true)
        {
            credits.gameObject.SetActive(clicked);
            mainMenu.gameObject.SetActive(false);
        }
        else
        {
            credits.gameObject.SetActive(clicked);
            mainMenu.gameObject.SetActive(true);
        }
    }
    public void restart(string name)
    {
        Application.LoadLevel(Application.loadedLevel);                                                  
    }
}

Shrine Script

using UnityEngine;
using System.Collections;

public class Shrine : MonoBehaviour
{
    
    Animator ThisAnimator;
    Collider2D collider;
    void Start()
    {
        collider = GetComponent<Collider2D>();
        ThisAnimator = GetComponent<Animator>();
    }
    void OnCollisionEnter2D(Collision2D collider)
    {
        if (collider.gameObject.tag == "Player")
        {   
            ThisAnimator.SetBool("usedShrine", true);
        }
    }
}


Player

using UnityEngine;
using System.Collections;

public class PlayerController : MonoBehaviour {
    public float moveSpeed;
    Rigidbody2D myRigidbody;

    public bool playerMoving;
    private Vector2 lastMove;
    public bool canMove;
    public Collider2D coll;
    float horiz, vert;
    public GameObject Shrine;
    int shrineCounter = 0;

    Animator thisAnimator;

	// Use this for initialization
	void Start () {
        myRigidbody = GetComponent<Rigidbody2D>();

        thisAnimator = GetComponent<Animator>();

        coll = GetComponent<Collider2D>();

	}

    void FixedUpdate()
    {
        horiz = Input.GetAxisRaw("Horizontal");
        vert = Input.GetAxisRaw("Vertical");

        MoveCharacter(horiz, vert);
    }

    void MoveCharacter(float xDir, float yDir)
    {


       
        myRigidbody.velocity = new Vector2(xDir * moveSpeed, yDir * moveSpeed);

        thisAnimator.SetFloat("Speed", myRigidbody.velocity.sqrMagnitude);
    }

    // Update is called once per frame
    void Update () {
    } 
    void OnCollisionEnter2D(Collision2D col)
    {
         if (col.gameObject.tag == "Shrine" && shrineCounter < 4)
         {
            Debug.Log("hi");
            shrineCounter++;
            thisAnimator.SetBool("ShrineTouch" + shrineCounter, true);
            col.gameObject.tag = "TouchedShrine";
          
         }

     }

}


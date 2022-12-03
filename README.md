# LR-27[LR-27.zip](https://github.com/Maxsim2418/LR-27/files/10146083/LR-27.zip)

Борнеман М.А

ЭВТ-70

Игровой движок:Unity

Лабораторная работа №27

Тема: Разработка игрового проекта пинг-понг

Цель: приобрести навыки в разработке игрового проекта пинг-понг

Ход работы:

1.	Выполнение работы

1.	Создание папок Fonts, Materials, Scenes, Scripts, Sprites.

![image](https://user-images.githubusercontent.com/119674602/205433963-1d679a08-8467-4dbb-95e1-0f0028744ccc.png)

Рис. 27.1. Создание папок

2.	Параметр Bouncy

![image](https://user-images.githubusercontent.com/119674602/205433969-2dccc875-2449-46d0-b78a-62c0a50db2a5.png)

Рис.27.2  Параметры Bouncy

![image](https://user-images.githubusercontent.com/119674602/205433975-ce22d74f-552c-4072-9826-a74737948b96.png)

Рис.27.3 – Параметры Bouncy

3.	Параметр Player paddle

![image](https://user-images.githubusercontent.com/119674602/205433980-182c144a-cad3-4bcc-9452-50b650e2be09.png)

Рис. 27.4. Настройка Player paddle

4.	Параметр Computer paddle

![image](https://user-images.githubusercontent.com/119674602/205433984-64cbdea8-df4f-4433-bd83-e3b9b663c79b.png)

Рис. 27.5. Настройка Computer paddle

5.	Параметр Top Wall

![image](https://user-images.githubusercontent.com/119674602/205433985-be600d76-efcd-4536-97ba-3428789ea00d.png)

Рис. 27.6. Настройка Top Wall

6.	Параметр Bottom Wall

![image](https://user-images.githubusercontent.com/119674602/205433988-30a0bb50-00ca-4ff6-afc0-f91d811cad56.png)

Рис. 27.7. Настройка Bottom Wall

7.	Параметр Left Wall

![image](https://user-images.githubusercontent.com/119674602/205433993-309dd286-29c6-4a0d-99db-b9e46df37631.png)

Рис. 27.8. Настройка Left Wall

8.	Параметр Right Wall

![image](https://user-images.githubusercontent.com/119674602/205434004-2005ff07-1e49-4258-bb1f-55c23e4c362f.png)

Рис. 27.9. Настройка Right Wall

9.	Параметр Line

![image](https://user-images.githubusercontent.com/119674602/205434010-67a1f026-1b63-45c4-b985-d14125cedb1e.png)

Рис 27.10. Настройка параметра Line

10.	Параметр GameManager

![image](https://user-images.githubusercontent.com/119674602/205434015-4246445a-3915-4c45-8879-e1dbf1ed01f3.png)

Рис. 27.11. Настройка параметра GameManager

11.	Параметр EventSystem

![image](https://user-images.githubusercontent.com/119674602/205434024-7b69d18b-061e-4a92-bc3b-3d9d6e78a875.png)

Рис. 27.12. Настройка параметра EventSystem

12.	Параметры Canvas и его дочерние параметры

![image](https://user-images.githubusercontent.com/119674602/205434032-90d0b41e-d822-4ed9-a290-d0e32fb83ce1.png)

Рис. 27.13. Параметры Canvas и его дочерние параметры

![image](https://user-images.githubusercontent.com/119674602/205434036-90f4a910-10d3-4019-b3a9-368b4f54f2dd.png)

Рис. 27.14. Параметры playerscore

![image](https://user-images.githubusercontent.com/119674602/205434038-c0e3424d-c9bd-4270-bdd5-f161af23614c.png)

Рис. 27.15. Параметры Computerscore

1.	Создал скрипт Ball

Листинг скрипта Ball.cs

public class Ball : MonoBehaviour
{
    public float speed = 300.0f;
    private Rigidbody2D _rigidbody;
    private void Awake()
    {
        _rigidbody = GetComponent<Rigidbody2D>();
    }	
    void Start()
    {
        ResetPosition();
        AddStartingForce();
    }
    public void ResetPosition()
    {
        _rigidbody.position = Vector3.zero;
        _rigidbody.velocity = Vector3.zero;
    }
    public void AddStartingForce()
    {
        float x = Random.value < 0.5f ? -0.1f : 1.0f;
        float y = Random.value < 0.5f ? Random.Range(-1.0f, -0.5f) :
                                        Random.Range(0.5f, 1.0f);
        Vector2 direction = new Vector2(x, y);
        _rigidbody.AddForce(direction * this.speed);
    }
    public void AddForce(Vector2 force)
    {
        _rigidbody.AddForce(force);
    }
}

2.	Создал скрипт BallSurface

Листинг скрипта BallSurface.cs

using UnityEngine;
public class BallSurface : MonoBehaviour
{
    public float bounceStrength;
    private void OnCollisionEnter2D(Collision2D collision)
    {
        Ball ball = collision.gameObject.GetComponent<Ball>();
        if (ball != null)
        {
            Vector2 normal = collision.GetContact(0).normal;
            ball.AddForce(-normal * this.bounceStrength);
        }
    }
}

3.	Создал скрипт ComputerPaddle 

Листинг скрипта ComputerPaddle.cs

public class ComputerPaddle : Paddle
{
    public Rigidbody2D ball;
    private void FixedUpdate()
    {
        if (this.ball.velocity.x > 0.0f)
        {
            if (this.ball.position.y > this.transform.position.y)
            {
                _rigidbody.AddForce(Vector2.up * this.speed);
            } 
            else if (this.ball.position.y < this.transform.position.y)
            {
                _rigidbody.AddForce(Vector2.down * this.speed);
            }
        }
        else
        {
            if (this.transform.position.y > 0.0f)
            {
                _rigidbody.AddForce(Vector2.down * this.speed);
            }
            else if (this.transform.position.y < 0.0f)
            {
                _rigidbody.AddForce(Vector2.up * this.speed);
            }
        }
    }
}

4.	Создал скрипт GameManager 

Листинг скрипта GameManager.cs

public class GameManager : MonoBehaviour
{
    public Ball ball;
    public Paddle computerPaddle;
    public Paddle playerPaddle;
    public TextMeshProUGUI PlayerScoreText;
    public TextMeshProUGUI computerScoreText;
    private int _playerScore;
    private int _computerScore;
    public void PlayerScores()
    {
        _playerScore++;
        this.PlayerScoreText.text = _playerScore.ToString();
        ResetRound();
       
    }
    public void ComputerScores()
    {
        _computerScore++;
        this.computerScoreText.text = _computerScore.ToString();
        ResetRound();
    }
    private void ResetRound()
    {
        this.computerPaddle.ResetPosition();
        this.playerPaddle.ResetPosition();
        this.ball.ResetPosition();
        this.ball.AddStartingForce();
    }
}

5.	Создал скрипт Paddle 

Листинг скрипта Paddle.cs

public class Paddle : MonoBehaviour
{
    public float speed = 10.0f;
    protected Rigidbody2D _rigidbody;
    private void Awake()
    {
        _rigidbody = GetComponent<Rigidbody2D>();
    }
    public void ResetPosition()
    {
        _rigidbody.position = new Vector2(_rigidbody.position.x, 0.0f);
        _rigidbody.velocity = Vector2.zero;
    }
}

6.	Создал скрипт PlayerPaddle 

Листинг скрипта PlayerPaddle.cs

public class PlayerPaddle : Paddle
{
    private Vector2 _direction;
    private void Update()
    {
        if (Input.GetKey(KeyCode.W) || Input.GetKey(KeyCode.UpArrow))
        {
            _direction = Vector2.up;
        }
        else if (Input.GetKey(KeyCode.S) || Input.GetKey(KeyCode.DownArrow))
        {
            _direction = Vector2.down;
        }
        else
        {
            _direction = Vector2.zero;
        }
    }
    private void FixedUpdate()
    {
        if (_direction.sqrMagnitude != 0)
        {
            _rigidbody.AddForce(_direction * this.speed);
        }
    }
}

7.	Создал скрипт ScoringZone 

Листинг скрипта ScoringZone.cs

public class ScoringZone : MonoBehaviour
{
    public EventTrigger.TriggerEvent scoreTrigger;
    private void OnCollisionEnter2D(Collision2D collision)
    {
        Ball ball = collision.gameObject.GetComponent<Ball>();
        if (ball != null)
        {
            BaseEventData eventData = new BaseEventData(EventSystem.current);
            this.scoreTrigger.Invoke(eventData);
        }
    }
}

2.	Вывод

В ходе выполненной работы была разработана игра ping pong

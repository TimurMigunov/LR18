# LR18

Мигунов Т.У.

ЭВТ-70

Игровой Движок: Unity.

Лабораторная работа №18

Тема: Разработка игрового проекта змейка.

Цель: приобрести навыки в разработке игрового проекта змейка

Ход работы:

1.	Выполнение работы

1. Добавление спрайтов еды и змейки.
 
____________________![image](https://user-images.githubusercontent.com/119228138/204997873-821b8d18-5b92-4449-9bd7-69e911db2e3b.png)

 
____________________Рисунок 18.1 спрайты змейки и еды

2.скрипт для передвижения, Snake.
```
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(BoxCollider2D))]
public class Snake : MonoBehaviour
{
    private List<Transform> segments = new List<Transform>();
    public Transform segmentPrefab;
    public Vector2 direction = Vector2.right;
    private Vector2 input;
    public int initialSize = 4;

    private void Start()
    {
        ResetState();
    }

    private void Update()
    {

        if (direction.x != 0f)
        {
            if (Input.GetKeyDown(KeyCode.W) || Input.GetKeyDown(KeyCode.UpArrow))
            {
                input = Vector2.up;
            }
            else if (Input.GetKeyDown(KeyCode.S) || Input.GetKeyDown(KeyCode.DownArrow))
            {
                input = Vector2.down;
            }
        }

        else if (direction.y != 0f)
        {
            if (Input.GetKeyDown(KeyCode.D) || Input.GetKeyDown(KeyCode.RightArrow))
            {
                input = Vector2.right;
            }
            else if (Input.GetKeyDown(KeyCode.A) || Input.GetKeyDown(KeyCode.LeftArrow))
            {
                input = Vector2.left;
            }
        }
    }

    private void FixedUpdate()
    {

        if (input != Vector2.zero)
        {
            direction = input;
        }


        for (int i = segments.Count - 1; i > 0; i--)
        {
            segments[i].position = segments[i - 1].position;
        }


        float x = Mathf.Round(transform.position.x) + direction.x;
        float y = Mathf.Round(transform.position.y) + direction.y;

        transform.position = new Vector2(x, y);
    }

    public void Grow()
    {
        Transform segment = Instantiate(segmentPrefab);
        segment.position = segments[segments.Count - 1].position;
        segments.Add(segment);
    }

    public void ResetState()
    {
        direction = Vector2.right;
        transform.position = Vector3.zero;


        for (int i = 1; i < segments.Count; i++)
        {
            Destroy(segments[i].gameObject);
        }


        segments.Clear();
        segments.Add(transform);


        for (int i = 0; i < initialSize - 1; i++)
        {
            Grow();
        }
    }

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.gameObject.CompareTag("Food"))
        {
            Grow();
        }
        else if (other.gameObject.CompareTag("Obstacle"))
        {
            ResetState();
        }
    }

}

3.	Был создан скрипт для собирания еды змейкой.
Листинг 18.2 Food.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Food : MonoBehaviour
{
    public BoxCollider2D gridArea;

    private void Start()
    {
        RandomizePosition();
    }

    private void RandomizePosition()
    {
        Bounds bounds = this.gridArea.bounds;

        float x = Random.Range(bounds.min.x, bounds.max.x);
        float y = Random.Range(bounds.min.y, bounds.max.y);

        this.transform.position = new Vector3(Mathf.Round(x), Mathf.Round(y), 0.0f);
    }


    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.tag == "Player") { 
            RandomizePosition();
        }
    }
}
```

4.	Были созданы сегменты для увеличения змейки.
 
____________________![image](https://user-images.githubusercontent.com/119228138/204997913-ed611416-0a48-4b00-bf53-8be2dd13a049.png)

 
____________________Рисунок 18.2 сегменты змейки

5.	Были созданы стены, GameTag  “Obstacle” и привязан “Obstacle” к стенам змейке.
 
 ________________![image](https://user-images.githubusercontent.com/119228138/204997938-b58fc75a-7384-4666-bfc0-61c1a1afda18.png)

 
____________________Рисунок 18.3 кадр из игры

Вывод: В ходе проделанной работы был разработан игровой проект змейка.
[ЛР18. Разработка игры Snake (1).zip](https://github.com/TimurMigunov/LR18/files/10130411/18.Snake.1.zip)

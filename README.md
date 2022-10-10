АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнила:

Дементьева Юлия Дмитриевна
РИ-211002

Отметка о выполнении заданий (заполняется студентом):

Задание
Выполнение
Баллы

Задание 1	*	60

Задание 2	#	20

Задание 3	#	20

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:

к.т.н., доцент Денисов Д.В.
к.э.н., доцент Панов М.А.
ст. преп., Фадеев В.О.
N|Solid

Build Status

Структура отчета

Данные о работе: название работы, фио, группа, выполненные задания.
Цель работы.
Задание 1.

Код реализации выполнения задания. 

Визуализация результатов выполнения (если применимо).

Выводы.

Цель работы
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity

Задание 1
Реализовала совместную работу и передачу данных в связке Python - Google-Sheets – Unity. При выполнении задания использовала видео-материалы и исходные данные, предоставленные преподавателем курса.

Ход работы:

1. В облачном сервисе google console подключила API для работы с google sheets и google drive.

2. Реализовал запись данных из скрипта на python в google-таблицу. Данные описывают изменение темпа инфляции на протяжении 11 отсчётных периодов, с учётом стоимости игрового объекта в каждый период. ![2022-10-10 (2)](https://user-images.githubusercontent.com/114353535/194853632-6dc6b7f6-0a41-4257-a704-2b5e774c43e2.png)

![2022-10-10 (4)](https://user-images.githubusercontent.com/114353535/194854188-ee75f00e-5a4a-4d40-ae3b-073c86bd443f.png)


3. Написала функционал на Unity, в котором будет воспризводиться аудио-файл в зависимости от значения данных из таблицы.

using System.Collections;

using System.Collections.Generic;

using UnityEngine;

using UnityEngine.Networking;

using SimpleJSON;


public class NewBehaviourScript : MonoBehaviour

{

public AudioClip goodSpeak;

public AudioClip normalSpeak;

public AudioClip badSpeak;

private AudioSource selectAudio;

private Dictionary<string, float> dataSet = new Dictionary<string, float>();

private bool statusStart = false;

private int i = 1;



  // Start is called before the first frame update
  
  
  void Start()
  
  {
  
  StartCoroutine(GoogleSheets());
  
  }
  
  

  
  // Update is called once per frame
  
  void Update()
  
  {
  
  if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
  
  {
  
  
  StartCoroutine(PlaySelectAudioGood());
  
  Debug.Log(dataSet["Mon_" + i.ToString()]);
  
  }
  
  
  if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
  
  {
  
  StartCoroutine(PlaySelectAudioNormal());
  
  Debug.Log(dataSet["Mon_" + i.ToString()]);
  
  }
  
  if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
  
  {
  
  StartCoroutine(PlaySelectAudioBad());
  
  Debug.Log(dataSet["Mon_" + i.ToString()]);
  
  }
  
  }
  
  



IEnumerator GoogleSheets()

{

UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1I4CorKG18vvYRwGI53U-gwIgIA6PzjLE9tkd62hfTT8/values/Лист1?
key=AIzaSyCcIhSeQ5BwbM2aH3HE3tWKBybRuMilaCM");

yield return curentResp.SendWebRequest();

string rawResp = curentResp.downloadHandler.text;

var rawJson = JSON.Parse(rawResp);

foreach (var itemRawJson in rawJson["values"])

{

var parseJson = JSON.Parse(itemRawJson.ToString());

var selectRow = parseJson[0].AsStringList;

dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));

}

//Debug.Log(dataSet["Mon_1"]);

}

IEnumerator PlaySelectAudioGood()

{

statusStart = true;

selectAudio = GetComponent<AudioSource>();

  selectAudio.clip = goodSpeak;
  
  
  
  selectAudio.Play();
  
  yield return new WaitForSeconds(3);
  
  statusStart = false;
  
  i++;
  
  }
  
  IEnumerator PlaySelectAudioNormal()
  
  {
  
  
  statusStart = true;
  
  selectAudio = GetComponent<AudioSource>();
  
  selectAudio.clip = normalSpeak;
  
  selectAudio.Play();
  
  yield return new WaitForSeconds(3);
  
  statusStart = false;
  
  i++;
  
  }
  
  IEnumerator PlaySelectAudioBad()
  
  {
  
  statusStart = true;
  
  selectAudio = GetComponent<AudioSource>();
  
  selectAudio.clip = badSpeak;
  
  selectAudio.Play();
  
  yield return new WaitForSeconds(4);
  
  statusStart = false;
  
  i++;
  
  }
}
  

  
4.
  Создала новый проект на Unity, который будет получать данные из google-таблицы, в которую были записаны данные в предыдущем пункте.

![
  2022-10-10 (8)](https://user-images.githubusercontent.com/114353535/194858133-74449eab-9afd-4a5b-8fa1-6e17e6c708a7.png)

![2022-10-10 (11)](https://user-images.githubusercontent.com/114353535/194858568-9c8e9636-c3c4-497e-9a57-9dac09a530c7.png)


Выводы
  
В результате проделанной работы я познакомилась с программными средствами для организции передачи данных между инструментами google, Python и Unity.

Powered by
BigDigital Team: Denisov | Fadeev | Panov

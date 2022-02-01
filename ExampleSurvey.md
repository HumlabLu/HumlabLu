
## Survey config 
<img src="https://img.shields.io/badge/json-5E5C5C?style=for-the-badge&logo=json&logoColor=white" />

### config example
This is a simple survey config that contains only the basics: 
A header, a question and a footer.

    {
      "title": "Lego 1!",
      "name": "lego1",
      "id": "lego1",
      "publishNotificationTitle": "👑 Hey Champ",
      "publishNotificationBody": "🍃 There's a new assignment for you.",
      "expireNotificationTitle": "Don't drop this 👑",
      "expireNotificationBody": "🧱 Survey time's soon up.",
      "questions": [
        {
          "index": 0,
          "title": "Lego!",
          "text": "Velkommen",
          "type": "header"
        },
        {
          "index": 1,
          "text": "What does Lego mean?",
          "type": "single",
          "values": [
            "Play Well",
            "Leg Godt",
            "I play",
            "I construct"
          ]
        },
        {
          "index": 2,
          "type": "footer",
          "text": "Mange takk"
        }
      ]
    }

The result of this config is as follows

Push notification     |  Header	|  Question	|  Footer
:------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
![](https://i.ibb.co/9rz1BFL/IMG-0518.jpg)  |  ![](https://i.ibb.co/10zJnV0/IMG-0515.jpg) |  ![](https://i.ibb.co/Fmf7c8c/IMG-0516.jpg) |  ![](https://i.ibb.co/3fX9x0G/IMG-0517.jpg)

---

### Question types
The following question types can be used in the survey


 | | Type      | Code |
| ----------- | ----------- | ----------- |
 | no icon| Header      | header |
| <img src="https://i.ibb.co/3NzGq48/lta-single.png" width="50" height="50"> | Single multiple answer      | single |
| <img src="https://i.ibb.co/FYcKzqh/lta-multi.png" width="50" height="50">| Multiple choice   | multi  |
| <img src="https://i.ibb.co/FHP1Fgw/lta-likert.png" width="50" height="50">| Likert scale   | likert  |
| <img src="https://i.ibb.co/N1Dr84b/lta-open.png" width="50" height="50">| Open ended text responses   | open  |
| <img src="https://i.ibb.co/r6LybHd/lta-blanks.png" width="50" height="50">| Fill in the blank   | blanks  |
| <img src="https://i.ibb.co/D7H9rbv/lta-slider.png" width="50" height="50">| Slider scale   | slider  |
| <img src="https://i.ibb.co/7g1xwdh/lta-duration.png" width="50" height="50">| Time duration   | duration  |
| <img src="https://i.ibb.co/16Zqv64/lta-footer.png" width="50" height="50">| Footer      | footer  |


---

**A longer example with different question types, include-If and skip logic**

    {
      "title": "All question types",
      "name": "all question types",
      "questions": [
        {
          "index": 0,
          "title": "All question types",
          "text": "This is a survey with all question types",
          "type": "header"
        },
        {
          "values": [
            "Yes",
            "No"
          ],
          "text": "Have you talked to anyone today?",
          "type": "single"
        },
        {
          "values": [
            "Family",
            "Friends",
            "Instructors",
            "Random person in bar"
          ],
          "index": 2,
          "text": "Who were you speaking to?",
          "type": "multi"
        },
        {
          "values": [],
          "index": 3,
          "text": "How did it go?",
          "description": "",
          "minAnnotation": "1: Not so good",
          "maxAnnotation": "5: Very good",
          "type": "likert"
        },
        {
          "values": [
            "French",
            "English",
            "German",
            "Other"
          ],
          "skip": {
	        "ifChosen": 3,
	        "goto": 9
	      },
          "index": 4,
          "text": "What language did you speak?",
          "type": "multi"
        },
        {
          "includeIf": {
            "ifIndex": 4,
            "ifValue": 0
          },
          "index": 5,
          "text": "What did you talk about in French?",
          "type": "open"
        },
        {
          "includeIf": {
            "ifIndex": 4,
            "ifValue": 1
          },
          "values": [],
          "index": 6,
          "text": "What did you talk about in English?",
          "type": "open"
        },
	    {
          "includeIf": {
            "ifIndex": 4,
            "ifValue": 1
          },
	      "index": 7,
	      "type": "duration",
	      "text": "How long did you speak English?"
	    },
        {
          "values": [
            "joy",
            "ambiguous",
            "anger"
          ],
          "index": 8,
          "text": "When I use LTA I feel _____",
          "type": "blanks"
        },
        {
          "index": 9,
          "type": "slider",
          "text": "Do you like LTA?",
          "minAnnotation": "Not at all",
          "maxAnnotation": "Very much"
        },
        {
          "index": 10,
          "text": "That's all!",
          "type": "footer"
        }
      ]
    }

> A survey **must** start with a header and end with a footer.
All objects in a survey **must** contain an index, header is index 0.
Skip precedes includeIf.

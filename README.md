# ecoBOT1311
 import requests
import vk_api
import random
import json
from vk_api.longpoll import VkLongPoll, VkEventType
 
d = dict()
 
key = "Нижний Новгород" 
d[key] = ["улица, Московское ш., 104"] 
d[key].append("Московское ш., 302/2") 
d[key].append("просп. Гагарина, 176 в") 
d[key].append("ул. Коновалова, 4") 
d[key].append("Донбасская ул., 52") 
d[key].append("ул. Ульянова, 10б")
 
key = 'Дзержинск' 
d[key] = [] 
d[key].append('г. Дзержинск, ул. Речная 2а') 
d[key].append('г. Дзержинск, Игумновское шоссе 21') 
d[key].append('г. Дзержинск, ул. Чкалова, д. 23') 
d[key].append('г. Дзержинск, пр-т Ленина, д.66')
 
 
with open("data.json", "w") as outfile:
    json.dump(d, outfile)
 
 
vk_session = vk_api.VkApi(token='d6e7c58a027772741cb310a104bf304f34adb40a3b92705ffa2db5ae3fac18252e01ef00fdc7954fd47f1')
 
with open("data.json", "r") as readfile:
   data = json.load(readfile)
 
longpoll = VkLongPoll(vk_session)
vk = vk_session.get_api()
for event in longpoll.listen():
    if event.type == VkEventType.MESSAGE_NEW and event.to_me and event.text:
   #Слушаем longpoll, если пришло сообщение то:     
        req = event.text
        
        comm = req.find(' ')
        
 
        if req[0 : comm]  == "сборбатареек":
            if event.from_user: #Если написали в ЛС
            
                key = req[comm + 1 : ]
                
                vk.messages.send(
                        user_id=event.user_id,
                        message = str(data[key]),
                        random_id=random.randint(1, 2000))
                    
        else:
            vk.messages.send(
                    user_id=event.user_id,
                    message = "мала иле многа аргументов",
                    random_id=random.randint(1, 2000))

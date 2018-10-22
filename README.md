# Twitterbot_for_slack
setting up a twitter bot to monitor multiple twitter account in real time and get real time notification in slack

<img width="657" alt="screen shot 2018-10-22 at 2 47 10 pm" src="https://user-images.githubusercontent.com/8762047/47314343-6ad50900-d60f-11e8-981c-8a55a6da2b32.png">

IF you are reading this blog then you probably looking for developing some kind of tweet monitor bot for slack. In this article i am going to describe how i created a dynamic tweet monitor bot in python to monitor clients if they are tweeting about something particular. suppose you want to monitor one of your client about when they tweet about something in particular. If you want to monitor just one account then its fine but think about a situation when you are monitoring 100 of twitter accounts. You are definitely going to miss the important tweets in that tweet flood in your slack channel. Suppose you are monitoring Zara and wants to know when they are tweeting about sale. you just want a slack notification when all those big companies like Zara, Columbia, North face etc will be tweeting about sale. That could be any kind of company. So, you don't want to write 50 scripts for 50 companies instead you will create one dynamic script which will be doing the job for all the clients. Now, what if you want to add or remove certain customers from your watch list? we won;'t touch the script but we will be maintaining a CSV file or any kind of database and it will serve the purpose.

1.) First we need few details like below which can be created by going on twitter and follow the instruction on https://chimpgroup.com/knowledgebase/twitter-api-keys/
# Consumer:
CONSUMER_KEY = 'consumer_key'
CONSUMER_SECRET = 'consumer_secret'
# Access:
ACCESS_TOKEN = 'access_token'
ACCESS_SECRET = 'access_secret'

2.) To interact with slack we need to create a slack bot API which you can create by following instruction on the below youtube video :-
https://www.youtube.com/watch?v=nyyXTIL3Hkw

SLACK API = 'xoxb-gfgdfgdfgdgfdfg'
you will get your slack api something like above starting with xoxb.

3.) we will also be maintaining a csv file in which we will be maintaining client twitter name information in the format like below:-
<img width="153" alt="screen shot 2018-10-22 at 3 05 50 pm" src="https://user-images.githubusercontent.com/8762047/47314344-6ad50900-d60f-11e8-8452-6d629472887b.png">

4.) we need some libraries to set up the whole process. So, please install the below libraries into your environment:-

<img width="857" alt="screen shot 2018-10-22 at 3 09 37 pm" src="https://user-images.githubusercontent.com/8762047/47314346-6ad50900-d60f-11e8-87e1-187aec11120c.png">
5.) After that we will declare the secret keys of the twitter developer account and slack API. you can use any account but if you want to use Vivox's detail, please contact JIM. you can always create slack API on your own and i have shared the link above for taking reference on how to create a slack API key.
<img width="552" alt="screen shot 2018-10-22 at 3 10 53 pm" src="https://user-images.githubusercontent.com/8762047/47314347-6ad50900-d60f-11e8-9900-972b7170e9b2.png">

6.) Then we will read the CSV file into a panda's dataframe. The CSV file is having all the details of the customer twitter name details and if you need to add or delete any twitter name, you can do that in the CSV file and there will be no need to edit the python script.

<img width="334" alt="screen shot 2018-10-22 at 3 11 46 pm" src="https://user-images.githubusercontent.com/8762047/47314350-6ad50900-d60f-11e8-8c11-a8a95e2ebc3b.png">

7.)Then we will define the tweepy function which we will use as extractor later :-
<img width="555" alt="screen shot 2018-10-22 at 3 12 14 pm" src="https://user-images.githubusercontent.com/8762047/47314351-6ad50900-d60f-11e8-8c89-dd481c8ed94f.png">

8. ) Now, we will create a datafrme "data" :-
<img width="604" alt="screen shot 2018-10-22 at 3 13 50 pm" src="https://user-images.githubusercontent.com/8762047/47314352-6ad50900-d60f-11e8-81db-1941a59e998b.png">

9.) Now, Twitter provides various details with every tweet. So, If you want to see what all details every tweet is having you can use the below code and see
<img width="1194" alt="screen shot 2018-10-22 at 3 14 22 pm" src="https://user-images.githubusercontent.com/8762047/47314353-6b6d9f80-d60f-11e8-9eda-6a6b6c2edfa9.png">

10.) Now, whatever information you want to extract and add as a column into the dataframe with the below code. I just have added three details but you can add as much as you want.
<img width="588" alt="screen shot 2018-10-22 at 3 15 02 pm" src="https://user-images.githubusercontent.com/8762047/47314354-6b6d9f80-d60f-11e8-8c75-839cdff31d7d.png">

11.) We do not want any tweet which is done by any user or if our client in tweeting in response to any user's tweet. To ignore all that kind of tweets, use below code:-

<img width="428" alt="screen shot 2018-10-22 at 3 15 42 pm" src="https://user-images.githubusercontent.com/8762047/47314355-6b6d9f80-d60f-11e8-9a85-8cdcd36db8c9.png">

12.) Now, we do not want the same tweet as notification twice so I am restriction my tweet window to check for the last 1 minute tweet only and the cron job is running every minute. The issue here is twitter data is in different time zone and our system time zone is different. So, I am changing both the time in a common format which is UTC.

<img width="794" alt="screen shot 2018-10-22 at 3 16 26 pm" src="https://user-images.githubusercontent.com/8762047/47314356-6b6d9f80-d60f-11e8-8bad-951897c42d50.png">

13.) After we are done with this step then we will be creating a list of values on the base of which we will filter the tweets as we wants tweets which is having certain words in it like downtime, maintenance etc.

<img width="886" alt="screen shot 2018-10-22 at 3 19 25 pm" src="https://user-images.githubusercontent.com/8762047/47314360-6b6d9f80-d60f-11e8-8dca-896f9d7e2360.png">

14.) In the last step we will just check if there is any data in the data frame "ndata" and send it as slack notification:-
<img width="867" alt="screen shot 2018-10-22 at 3 17 24 pm" src="https://user-images.githubusercontent.com/8762047/47314359-6b6d9f80-d60f-11e8-803b-ff18de7c20a8.png">


15.) Once you are done with all of teh above step, setup the cron job by going to your terminal and use command :-
     crontab -e
     and then make an entry like below 
     */1 * * * * /your_path/twitter.py

END Notes:-
) Make sure that any of the account name is not invalid or private.
) Modify your my_list as per your requirement.

Please let me know if you got any error in the comment section. Please add any valuable suggestion how i can even improve this script. I am graduate student at Northeastern University and currently working as a Data Scientist at Vivox.

Linkedin - https://www.linkedin.com/in/abhimanyu0301/

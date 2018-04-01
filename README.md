# Mongo-LATimes-News-Scrapper
<img src="https://user-images.githubusercontent.com/29084524/34926082-1cc4eb3e-f962-11e7-80b5-647e702c9ad7.gif" width="425"/> <img src="https://user-images.githubusercontent.com/29084524/34926147-83eb9920-f962-11e7-959b-a469926b6e89.gif" width="425"/>

There were these things called newspapers... and once upon a time, this was the main source of news.

But not anymore. Now we have the internet... and why bother going outside, putting pants on and having to sift and fumble through a newspaper?

Just scrape the LA Times!

<p align="center">
<img src="https://user-images.githubusercontent.com/29084524/34926085-21230b70-f962-11e7-9d15-3ac3983e7831.gif" width="425"/>
</p>

That's right, here you'll find all the latest articles. View them, comment on them and save your favorite articles for later.

## Instructions
Take a look at the live [demo](https://mysterious-reaches-59628.herokuapp.com/), scrape the LA Times and waste some time at work :)

## Requirements
In this assignment, you'll create a web app that lets users view and leave comments on the latest news. But you're not going to actually write any articles; instead, you'll flex your Mongoose and Cheerio muscles to scrape news from another site.

## Code Higlights

### Scraping Web Data using Cheerio
The crux of this app lies in being able to scrape a website for relavant information and none of the fluff. That's where the NPM package, [Cheerio](https://www.npmjs.com/package/cheerio), comes in handy. With it, I'm able to scrape the LA Times for the data that I need and by loading the website's html into a constant, I'm then able to execute a .each method which allows me to grab the information I want, store it into Mongodb by requiring my Results model into my controller. The resulting data is then passed into the handlebars view engine to be rendered to the client.
```
router.get("/scraped", function(req, res) {
request("http://www.latimes.com/latest/", function(error, response, html) {

const $ = cheerio.load(html);
let result = {}

$('li.trb_outfit_group_list_item').each((i, element) => {
result.link = (($(element).find('a').attr('href')).split(".html")[0]);
result.headline = $(element).find('h3').find('a').text().trim();
result.summary = $(element).find('p.trb_outfit_group_list_item_brief').text().trim();
result.img = $(element).find('a').find('img').attr('data-baseurl');
result.createdAt = $(element).find('span.trb_outfit_categorySectionHeading_date').attr('data-dt');

let entry = new Results(result);

Results.find({headline: result.headline}, (err, data) => {
// if (data.length === 0) {
entry.save((err, data) => {
if (err) throw err;
});
// }
});
});
console.log("success");
res.redirect("/");
});
});
```

## Built With
+ [Node.js](https://nodejs.org/en/)
+ [Express.js NPM Package](https://www.npmjs.com/package/express)
+ [Body Parser NPM Package](https://www.npmjs.com/package/body-parser)
+ [Express Handlebars NPM Package](https://www.npmjs.com/package/express-handlebars)
+ [Moment.js](http://momentjs.com/)
+ [MongoDB](https://www.mongodb.com/)
+ [Mongoose ODM](http://mongoosejs.com/)
+ [Bootstrap 4](https://getbootstrap.com/)
+ [MLab](https://mlab.com/)
+ [Request NPM Package](https://www.npmjs.com/package/request)
+ [Google Fonts](https://fonts.google.com/)
+ [Heroku](https://dashboard.heroku.com/)

### Authors
+ [Alex Edward Ball](https://github.com/AlexEBall)

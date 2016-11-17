---
layout: post
title: "Shuffling Arrays with Fisher–Yates"
date: "2016-08-27 16:38:19 -0800"
tags: [javascript, algorithm]
comments: true
---
Recently, on a project, I was tasked with the responsibility of randomizing an array of questions **and** the array of answers available for each question. At first, this sounded like a fairly simple task, but I soon discovered that it was a bit more involved.

First, the content:

{% highlight javascript %}
// Constructor for questions
class Quiz {
  constructor (questions) {
    this.questions = questions;
  }
}
// create the questions for the quiz
const questions = [{
   question: {
       text: 'Who is Mario\'s dinosaur friend?',
       answers: ['Rex', 'Luigi', 'Toshi', 'Yoshi'],
       correctAnswer: 'Yoshi'
   },
}, {
   question: {
       text: 'What does Sonic the Hedgehog love to collect?',
       answers: ['Jewels', 'Coins', 'Diamonds', 'Rings'],
       correctAnswer: 'Rings'
   },
}, {
   question: {
       text: 'Which of these is NOT a Pac-Man ghost?',
       answers: ['Inky', 'Clyde', 'Blinky', 'Bonnie'],
       correctAnswer: 'Bonnie'
   },
}, {
   question: {
       text: 'Who is Link\'s fairy companion?',
       answers: ['Siri', 'Alexa', 'Cortana', 'Navi'],
       correctAnswer: 'Navi'
   },
}, {
   question: {
       text: 'Which game has a character that says "@!#?@!" every time he is hit by something?',
       answers: ['Donkey Kong', 'Mega Man', 'Mario', 'Q*bert'],
       correctAnswer: 'Q*bert'
   },
}];

new Quiz(questions);
{% endhighlight %}

My first guess at dealing with this was to loop through the array of questions via a function along with applying some `Math.random()` to each array index then assigning those to a new array. This was a step in the right direction but there was a key component I was missing. After some research, I discovered the extremely useful [Fisher–Yates Shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle) which "is an algorithm for generating a random permutation of a finite set—in plain terms, the algorithm shuffles the set". Using this algorithm, we can create this function

{% highlight javascript %}
// Shuffle function using the Fisher–Yates Shuffle
function shuffle(array) {
    let counter = array.length;
    // While there are elements in the array
    while (counter) {
        // Pick a random index and decrease the counter
        let index = Math.floor(Math.random() * counter--);
        // Then swap the last element with it
        let temp = array[counter];
        array[counter] = array[index];
        array[index] = temp;
    }
    return array;
}
// Shuffle questions
let shuffledQuestions = shuffle(questions);
{% endhighlight %}

and pass in our array of questions to algorithmically shuffle them. This works really well, but we still have the problem of shuffling the answers associated with each question. To solve this, we can use a good ol' for loop on our `shuffledQuestions` and apply our shuffle function to the answers of each question.

{% highlight javascript %}
// Shuffle each question's answers
for (let i = 0; i < shuffledQuestions.length; i++) {
	shuffle(shuffledQuestions[i].question.answers);
}
{% endhighlight %}

This all makes for a great dynamic and unpredictable set of questions and answers useable for a quiz.

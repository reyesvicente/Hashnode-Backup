---
title: "Stuck in a Rut? Let This Random A Day to Remember Song Picker Spark Your Creativity!"
datePublished: Mon Feb 03 2025 06:11:57 GMT+0000 (Coordinated Universal Time)
cuid: cm6onlarp001s08js6z3b3ypo
slug: stuck-in-a-rut-let-this-random-a-day-to-remember-song-picker-spark-your-creativity
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738563088925/ecf4b33f-5db3-4f9f-bc18-26dbd6e04b79.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1738563104661/ba701746-fc12-4324-893d-23a8de8e89b3.webp
tags: music, javascript

---

Ever find yourself in a creative slump, unsure what song to cover next? As musicians, we all hit that wall sometimes. Staring at our instruments, hoping for inspiration to strike. Well, fellow musicians and A Day to Remember fans, I've got a little something that might just reignite your passion: a random A Day to Remember song picker!

I've created a simple webpage (you can even copy and paste the code below into a text file, save it as an HTML file, and open it in your browser!) that randomly selects a song from A Day to Remember's extensive discography. No more endless scrolling through their albums, agonizing over which track to tackle. Just a single click and BAM! You've got your next challenge.

## How Does It Work?

It's super easy. The page features a button that, when clicked, triggers a JavaScript function. This function randomly picks a song from a pre-defined list of A Day to Remember tracks (everything from "Intro" to "Last Chance to Dance (Bad Friend)"). The chosen song title is then displayed on the page.

## Why Did I Make This?

I'm a huge ADTR fan, and I often find myself wanting to learn or cover their songs. But sometimes, the sheer number of awesome tracks they have can be overwhelming. I wanted a quick and easy way to shake things up and push myself out of my comfort zone. Plus, I thought other musicians might find it useful too!

## Stuck in a Rut? Let This Random A Day to Remember Song Picker Spark Your Creativity!

Ever find yourself in a creative slump, unsure what song to cover next? As musicians, we all hit that wall sometimes. Staring at our instruments, hoping for inspiration to strike. Well, fellow musicians and A Day to Remember fans, I've got a little something that might just reignite your passion: a random A Day to Remember song picker!

I've created a simple webpage (you can even copy and paste the code below into a text file, save it as an HTML file, and open it in your browser!) that randomly selects a song from A Day to Remember's extensive discography. No more endless scrolling through their albums, agonizing over which track to tackle. Just a single click and BAM! You've got your next challenge.

**How Does It Work?**

It's super easy. The page features a button that, when clicked, triggers a JavaScript function. This function randomly picks a song from a pre-defined list of A Day to Remember tracks (everything from "Intro" to "Last Chance to Dance (Bad Friend)"). The chosen song title is then displayed on the page.

**Why Did I Make This?**

I'm a huge ADTR fan, and I often find myself wanting to learn or cover their songs. But sometimes, the sheer number of awesome tracks they have can be overwhelming. I wanted a quick and easy way to shake things up and push myself out of my comfort zone. Plus, I thought other musicians might find it useful too!

**How Can You Use It?**

* **Challenge Yourself:** Pick a song you've never played before and learn it. This is a great way to expand your musical vocabulary and improve your skills.
    
* **Break Out of a Rut:** If you're stuck playing the same old songs, this tool can help you discover hidden gems in ADTR's catalog.
    
* **Practice Different Styles:** A Day to Remember blends pop punk and metalcore, so this random picker can push you to explore different playing techniques.
    
* **Just Have Fun!** Ultimately, music should be enjoyable. This tool is a fun way to engage with ADTR's music and discover new favorites.
    

### The Code (For those who are interested):

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Random Song Picker</title>
</head>
<body>
    <h1>Random A Day to Remember Song Picker</h1>
    <button onclick="pickSong()">Pick a Song</button>
    <p id="song"></p>

    <script>
        const songs = [
            "Intro", "Heartless", "Your Way With Words Is Through Silence", "A Second Glance",
            "If Looks Could Kill", "The Danger in Starting a Fire", "Show 'Em the Ropes",
            "You Should've Killed Me When You Had the Chance", "Point of No Return", "Somewhere in Neverland",
            "Came Back Haunted", "The Downfall of Us All", "My Life for Hire", "The Danger in Starting a Fire",
            "A Shot in the Dark", "Start the Fire", "Mr. Highway's Thinking About the End", "Have Faith in Me",
            "Welcome to the Family", "Homesick", "Runnin' for Cover", "The Last Song", "Sticks & Bricks",
            "All I Want", "It's Complicated", "This Is the House That Doubt Built", "2nd Sucks",
            "Better Off This Way", "City of Ocala", "Right Back at It Again", "Sometimes You're the Hammer, Sometimes You're the Nail",
            "Dead & Buried", "The Document Speaks for Itself", "Rescue Me", "I Remember", "Violence (Enough Is Enough)",
            "Life Lessons Learned the Hard Way", "Bad Vibrations", "Paranoia", "Naivety", "Bullfight",
            "The Reaping", "Just Keep Breathing", "We Got This", "Same Book but Never the Same Page",
            "Turn Off the Radio", "Brick Wall", "Mindreader", "Resentment", "Bloodsucker",
            "Only Money", "Degenerates", "Last Chance to Dance (Bad Friend)"
        ];

        function pickSong() {
            const randomIndex = Math.floor(Math.random() * songs.length);
            document.getElementById("song").innerText = "Cover this song: " + songs[randomIndex];
        }
    </script>
</body>
</html>
```

So, what are you waiting for? Give the random A Day to Remember song picker a try and see what musical adventure awaits you! Let me know what song you get in the comments below! Happy playing!
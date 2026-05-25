# Transcript

- 言語: en
- ソース: YouTube auto-captions
- 話者分離: なし

## 本文

[00:00:01] Today we're breaking down the full AI design workflow from start to finish.
[00:00:03] design workflow from start to finish. We'll look at the current state of AI
[00:00:04] We'll look at the current state of AI tools in Figma, how to set everything
[00:00:06] tools in Figma, how to set everything up, when to use Claude, Codeex, Claude
[00:00:08] up, when to use Claude, Codeex, Claude Design, Figma, and more, and how to
[00:00:10] Design, Figma, and more, and how to combine them without wasting time or
[00:00:11] combine them without wasting time or burning through tokens. We'll also cover
[00:00:13] burning through tokens. We'll also cover how to train AI in your design system,
[00:00:15] how to train AI in your design system, generate original screens, improve the
[00:00:17] generate original screens, improve the quality of AI outputs, and build a
[00:00:18] quality of AI outputs, and build a workflow that actually feels usable for
[00:00:20] workflow that actually feels usable for real product design work. Before we
[00:00:22] real product design work. Before we begin, a couple quick things. If you're
[00:00:23] begin, a couple quick things. If you're a big fan of myself, what we're doing,
[00:00:25] a big fan of myself, what we're doing, consider checking out UI Collective
[00:00:26] consider checking out UI Collective Academy. Again, this is all just on our
[00:00:27] Academy. Again, this is all just on our website link in the description where
[00:00:28] website link in the description where you get access to a bunch of great
[00:00:29] you get access to a bunch of great courses on AI design systems and more.
[00:00:32] courses on AI design systems and more. Private Slack channel access me and the
[00:00:33] Private Slack channel access me and the team and always releasing new content on
[00:00:35] team and always releasing new content on this as well. Or if you don't want to
[00:00:37] this as well. Or if you don't want to support the academy, it's totally free
[00:00:38] support the academy, it's totally free to join the community. You can come ask
[00:00:39] to join the community. You can come ask any question for free on our forum. Also
[00:00:41] any question for free on our forum. Also get access to some great savings and
[00:00:43] get access to some great savings and resources too. So whether you want to
[00:00:44] resources too. So whether you want to join the community or support the
[00:00:45] join the community or support the academy, all this is just on our
[00:00:46] academy, all this is just on our website. Thanks for being here. Also,
[00:00:48] website. Thanks for being here. Also, come follow me on X. Link to my X along
[00:00:50] come follow me on X. Link to my X along with our website, both in the
[00:00:51] with our website, both in the description. Right off the bat, for
[00:00:53] description. Right off the bat, for designers, AI is not a tool. AI is a
[00:00:57] designers, AI is not a tool. AI is a workflow. Two to three years ago when
[00:01:00] workflow. Two to three years ago when Figma was all we were using, that's when
[00:01:02] Figma was all we were using, that's when design relied on one tool. That's it.
[00:01:05] design relied on one tool. That's it. All we were using was Figma. We weren't
[00:01:06] All we were using was Figma. We weren't using anything else. But now with the
[00:01:09] using anything else. But now with the introduction of AI, the ability to spin
[00:01:11] introduction of AI, the ability to spin up rapid iterations, spin up handoff
[00:01:14] up rapid iterations, spin up handoff documentation, do a lot of the manual
[00:01:16] documentation, do a lot of the manual work that we were doing before, using AI
[00:01:19] work that we were doing before, using AI is not just about knowing how to use one
[00:01:21] is not just about knowing how to use one tool. It's about knowing how to use
[00:01:23] tool. It's about knowing how to use multiple tools, how to collaborate
[00:01:25] multiple tools, how to collaborate between those tools, and how to adjust
[00:01:27] between those tools, and how to adjust your approach depending on what it is
[00:01:29] your approach depending on what it is that you're actually working on. So, as
[00:01:32] that you're actually working on. So, as designers, we're always looking for one
[00:01:33] designers, we're always looking for one tool to do everything that we were using
[00:01:36] tool to do everything that we were using Figma for before, but now it's about how
[00:01:39] Figma for before, but now it's about how we're using multiple tools to make
[00:01:41] we're using multiple tools to make ourselves a little bit more efficient.
[00:01:43] ourselves a little bit more efficient. Let's take a look at what that looks
[00:01:44] Let's take a look at what that looks like. And naturally, because Figma was
[00:01:46] like. And naturally, because Figma was our home for so long, we're looking for
[00:01:48] our home for so long, we're looking for a tool that does everything. And I
[00:01:50] a tool that does everything. And I actually got an email from someone
[00:01:52] actually got an email from someone recently whose boss was like really
[00:01:53] recently whose boss was like really putting pressure on them to find a tool
[00:01:55] putting pressure on them to find a tool that does exactly what I'm about to talk
[00:01:57] that does exactly what I'm about to talk about here, but doesn't exist. Um, so
[00:01:59] about here, but doesn't exist. Um, so what designers really looking for in one
[00:02:01] what designers really looking for in one tool is the ability to upload a design
[00:02:03] tool is the ability to upload a design system flawlessly where the AI
[00:02:05] system flawlessly where the AI automatically knows every variable,
[00:02:07] automatically knows every variable, every style, every component, when to
[00:02:09] every style, every component, when to use those components, variables, and
[00:02:11] use those components, variables, and styles. It is just an expert on
[00:02:14] styles. It is just an expert on everything right out of the gate. the
[00:02:16] everything right out of the gate. the ability to prompt our way to desired
[00:02:18] ability to prompt our way to desired designs on a canvas UI very similar to
[00:02:22] designs on a canvas UI very similar to Figma where we can select certain
[00:02:23] Figma where we can select certain elements, move things around, manually
[00:02:26] elements, move things around, manually adjust things if we need to. An option
[00:02:29] adjust things if we need to. An option to have Figma like functionality for
[00:02:30] to have Figma like functionality for finer adjustments like the ability to,
[00:02:33] finer adjustments like the ability to, you know, control the variables
[00:02:34] you know, control the variables globally, the ability to tweak spacing
[00:02:36] globally, the ability to tweak spacing just between a couple items.
[00:02:39] just between a couple items. uh one click to generate all handoff
[00:02:41] uh one click to generate all handoff documentation for both designers, for
[00:02:43] documentation for both designers, for developers, when to use components,
[00:02:45] developers, when to use components, accessibility guidelines, when to use
[00:02:47] accessibility guidelines, when to use specific pages, and then a one-click
[00:02:49] specific pages, and then a one-click build to make developers lives easy
[00:02:51] build to make developers lives easy where you just click build and it
[00:02:54] where you just click build and it generates, you know, perfect code for
[00:02:56] generates, you know, perfect code for the developers to go and implement. But
[00:02:59] the developers to go and implement. But it doesn't work like this. This is our
[00:03:01] it doesn't work like this. This is our dream and maybe in three years we're
[00:03:02] dream and maybe in three years we're going to get there in the future, but
[00:03:04] going to get there in the future, but this does not exist in current state
[00:03:06] this does not exist in current state right now. Instead, what it looks like
[00:03:08] right now. Instead, what it looks like is we need to train AI on our design
[00:03:10] is we need to train AI on our design system. We can't just upload it and
[00:03:12] system. We can't just upload it and expect AI to do everything. Is we need
[00:03:14] expect AI to do everything. Is we need to go through a series of prompts to
[00:03:15] to go through a series of prompts to make sure AI understands our current
[00:03:18] make sure AI understands our current brand, our styles, our variables, our
[00:03:20] brand, our styles, our variables, our components, and then store that
[00:03:22] components, and then store that knowledge somewhere so you don't need to
[00:03:24] knowledge somewhere so you don't need to keep telling it every time. We need to
[00:03:27] keep telling it every time. We need to generate multiple iterations across
[00:03:29] generate multiple iterations across tools because the output that you might
[00:03:31] tools because the output that you might get from something like claude is going
[00:03:32] get from something like claude is going to be different than the output that you
[00:03:33] to be different than the output that you might get from something like codec
[00:03:35] might get from something like codec codeex which is going to be different
[00:03:36] codeex which is going to be different than something that you might get from
[00:03:38] than something that you might get from Google stitch. So it's important to
[00:03:40] Google stitch. So it's important to generate multiple iterations across some
[00:03:42] generate multiple iterations across some of these tools and then when it comes to
[00:03:45] of these tools and then when it comes to the Figma integration or how Figma loops
[00:03:47] the Figma integration or how Figma loops into this is you can push and pull from
[00:03:51] into this is you can push and pull from Figma to different AI tools for
[00:03:53] Figma to different AI tools for iteration. So something might come out
[00:03:54] iteration. So something might come out in cloud code. I can push that to Figma,
[00:03:57] in cloud code. I can push that to Figma, make some tweaks in Figma, make the
[00:03:59] make some tweaks in Figma, make the styling, the layout tweaks that I want,
[00:04:01] styling, the layout tweaks that I want, and then bring that back into whatever
[00:04:03] and then bring that back into whatever AI tool that I was working with before.
[00:04:05] AI tool that I was working with before. So there's a little bit of give and take
[00:04:06] So there's a little bit of give and take there. And then build out with AI with
[00:04:09] there. And then build out with AI with the help of a developer. So it's not as
[00:04:11] the help of a developer. So it's not as easy as just one click builds everything
[00:04:13] easy as just one click builds everything perfectly. A perfect code, you have
[00:04:15] perfectly. A perfect code, you have everything that you want. You need
[00:04:17] everything that you want. You need developers are there for a reason. we
[00:04:19] developers are there for a reason. we can support them on some of these items,
[00:04:21] can support them on some of these items, but it's not as easy as just going from
[00:04:24] but it's not as easy as just going from prototype to production level code right
[00:04:27] prototype to production level code right away. And then we need to work with AI
[00:04:29] away. And then we need to work with AI to generate a lot of the documentation
[00:04:31] to generate a lot of the documentation if whether we're building design system
[00:04:32] if whether we're building design system components, you know, designs, you know,
[00:04:34] components, you know, designs, you know, different modules. AI doesn't
[00:04:37] different modules. AI doesn't automatically know the usability
[00:04:39] automatically know the usability guidelines, the accessibility guidelines
[00:04:41] guidelines, the accessibility guidelines for everything that we're spinning up.
[00:04:43] for everything that we're spinning up. We still need to dialogue with the AI in
[00:04:45] We still need to dialogue with the AI in order to get the specific outputs that
[00:04:47] order to get the specific outputs that we're looking for. And with this, this
[00:04:49] we're looking for. And with this, this is not just one tool, but this is a
[00:04:51] is not just one tool, but this is a suite of different tools in the AI space
[00:04:55] suite of different tools in the AI space that we need to know how to maneuver
[00:04:58] that we need to know how to maneuver through.
[00:04:59] through. How to generate with Claude, how to
[00:05:01] How to generate with Claude, how to generate with codeex, how to generate
[00:05:03] generate with codeex, how to generate with Google Stitch. Where does Figma
[00:05:04] with Google Stitch. Where does Figma come into play? How do we get the better
[00:05:06] come into play? How do we get the better results with each of these tools?
[00:05:08] results with each of these tools? Because as the AI landscape evolves, we
[00:05:10] Because as the AI landscape evolves, we can't just get away with just knowing
[00:05:12] can't just get away with just knowing one tool. If we just get away with
[00:05:14] one tool. If we just get away with knowing Google Stitch, then we go into
[00:05:16] knowing Google Stitch, then we go into an interview or all of a sudden we need
[00:05:17] an interview or all of a sudden we need to work with Claude or Codeex, then
[00:05:19] to work with Claude or Codeex, then we're behind other designers. So, you
[00:05:23] we're behind other designers. So, you need to be able to work with all of
[00:05:25] need to be able to work with all of them. It's not like you can just know
[00:05:26] them. It's not like you can just know Claude, just know Codeex. They're all
[00:05:29] Claude, just know Codeex. They're all going to make their own leaps and
[00:05:31] going to make their own leaps and innovation, especially in the design
[00:05:32] innovation, especially in the design space eventually. And you just saw that
[00:05:34] space eventually. And you just saw that with Claude Design. Codeex might come
[00:05:36] with Claude Design. Codeex might come out with something similar in the future
[00:05:38] out with something similar in the future that might be better than what Figma
[00:05:40] that might be better than what Figma offers and be better than what Claude
[00:05:43] offers and be better than what Claude offers. So, it's important to have a
[00:05:46] offers. So, it's important to have a full understanding of the entire
[00:05:49] full understanding of the entire endto-end workflow. For the topic of
[00:05:51] endto-end workflow. For the topic of Figma is there's a lot of clickbait
[00:05:53] Figma is there's a lot of clickbait that's out there where people like, "Oh,
[00:05:55] that's out there where people like, "Oh, Figma's finished. Figma is going to go
[00:05:56] Figma's finished. Figma is going to go bankrupt. Figma doesn't stand a chance."
[00:05:59] bankrupt. Figma doesn't stand a chance." despite the fact that their stock price
[00:06:01] despite the fact that their stock price is down like 85 plus percent of all
[00:06:05] is down like 85 plus percent of all time, it's not finished. It's still
[00:06:06] time, it's not finished. It's still going to be around, but they've fallen
[00:06:09] going to be around, but they've fallen behind in the AI space. You know,
[00:06:11] behind in the AI space. You know, Figma's AI is incredibly underwhelming.
[00:06:13] Figma's AI is incredibly underwhelming. Um, Figma's design system sync does not
[00:06:16] Um, Figma's design system sync does not work very well. You can sync your design
[00:06:19] work very well. You can sync your design system with Figma make and all of a
[00:06:21] system with Figma make and all of a sudden it's applying like your error
[00:06:22] sudden it's applying like your error states to absolutely everything which is
[00:06:25] states to absolutely everything which is weird because you would think that
[00:06:26] weird because you would think that Figma's make would be able to recognize
[00:06:29] Figma's make would be able to recognize that it's an error state because the
[00:06:31] that it's an error state because the variant is named error but it doesn't it
[00:06:33] variant is named error but it doesn't it doesn't work very well and their general
[00:06:34] doesn't work very well and their general AI just produces really generic results.
[00:06:36] AI just produces really generic results. You can get way better results in codeex
[00:06:38] You can get way better results in codeex and claude code and claude design and
[00:06:40] and claude code and claude design and Google stitch as well. So they've got
[00:06:42] Google stitch as well. So they've got some work to do there but for today
[00:06:44] some work to do there but for today these are the tools that we're going to
[00:06:46] these are the tools that we're going to go over. We're going to go over cloud.
[00:06:47] go over. We're going to go over cloud. We're gonna go over codecs. We're going
[00:06:48] We're gonna go over codecs. We're going to go over where Figma comes into play.
[00:06:50] to go over where Figma comes into play. We're going to go over Google Stitch as
[00:06:52] We're going to go over Google Stitch as well. A lot of these concepts I might
[00:06:54] well. A lot of these concepts I might have covered in other videos as well,
[00:06:55] have covered in other videos as well, but hoping this is sort of the one-stop
[00:06:57] but hoping this is sort of the one-stop shop uh for everyone. Everything else
[00:06:59] shop uh for everyone. Everything else besides these tools here, as of right
[00:07:01] besides these tools here, as of right now, it's just noise. There's a lot of
[00:07:04] now, it's just noise. There's a lot of startups trying to enter the AI design
[00:07:06] startups trying to enter the AI design space, but nothing that really comes
[00:07:07] space, but nothing that really comes close to what's here or where I've found
[00:07:10] close to what's here or where I've found value like baking it into my workflow.
[00:07:12] value like baking it into my workflow. So just something that you should be
[00:07:13] So just something that you should be aware of to make the most of this course
[00:07:16] aware of to make the most of this course this video is download codeex
[00:07:21] this video is download codeex download or have an account with claude
[00:07:23] download or have an account with claude preferably you have the desktop app and
[00:07:25] preferably you have the desktop app and also make sure you have a Google account
[00:07:26] also make sure you have a Google account so you can follow along with us with
[00:07:28] so you can follow along with us with Google stitch as well and try out all
[00:07:30] Google stitch as well and try out all these tools and I'll put links to these
[00:07:31] these tools and I'll put links to these in the description. You're going to see
[00:07:33] in the description. You're going to see that claude and codeex very similar.
[00:07:35] that claude and codeex very similar. Codeex is essentially OpenAI's answer to
[00:07:38] Codeex is essentially OpenAI's answer to cloud code, but both can produce really
[00:07:40] cloud code, but both can produce really good designs. There's some key
[00:07:41] good designs. There's some key fundamental differences that you need to
[00:07:42] fundamental differences that you need to be aware of. First off, cloud is the
[00:07:44] be aware of. First off, cloud is the better code. If you're working with
[00:07:46] better code. If you're working with developers, they're going to want to use
[00:07:47] developers, they're going to want to use cloud code. They're not going to want to
[00:07:48] cloud code. They're not going to want to use codec. I've chatted with one
[00:07:49] use codec. I've chatted with one developer, it was the designer was using
[00:07:51] developer, it was the designer was using codeex and then when the designer gave
[00:07:53] codeex and then when the designer gave them the designs, they had to redo a lot
[00:07:56] them the designs, they had to redo a lot of it inside of cloud code. So, it's not
[00:07:58] of it inside of cloud code. So, it's not like a onetoone both produce the exact
[00:08:00] like a onetoone both produce the exact same code. However, Codex uses about
[00:08:03] same code. However, Codex uses about three to four times fewer tokens for the
[00:08:05] three to four times fewer tokens for the same work as Claude. What this means is
[00:08:08] same work as Claude. What this means is that if we're designing with AI and
[00:08:11] that if we're designing with AI and iterating, Codeex might be the better
[00:08:14] iterating, Codeex might be the better option because it allows us to go a
[00:08:16] option because it allows us to go a little bit farther with the tokens that
[00:08:18] little bit farther with the tokens that we have available. If you're on a clawed
[00:08:20] we have available. If you're on a clawed paid plan, especially that base pro
[00:08:22] paid plan, especially that base pro plan, you're going to realize that even
[00:08:25] plan, you're going to realize that even though you're paying for it, you're
[00:08:26] though you're paying for it, you're often going to run out of tokens,
[00:08:28] often going to run out of tokens, especially as you're working with
[00:08:30] especially as you're working with larger, more complex designs. Something
[00:08:32] larger, more complex designs. Something you should know about. Claude is,
[00:08:35] you should know about. Claude is, however, is more accurate with using
[00:08:37] however, is more accurate with using Figma attributes when pushed to Figma.
[00:08:39] Figma attributes when pushed to Figma. Sorry, the wording here is a little bit
[00:08:40] Sorry, the wording here is a little bit off, but what I mean by that is if I'm
[00:08:43] off, but what I mean by that is if I'm working with a design inside of Claude
[00:08:46] working with a design inside of Claude and I have my Figma skills, Figma MCP
[00:08:48] and I have my Figma skills, Figma MCP connected, which we're going to get to a
[00:08:49] connected, which we're going to get to a little bit later, and I push it to
[00:08:51] little bit later, and I push it to Figma, it's generally a little bit
[00:08:53] Figma, it's generally a little bit better about using auto layout, using
[00:08:56] better about using auto layout, using the responsiveness properties like fill,
[00:08:58] the responsiveness properties like fill, hug, than codeex. Others might
[00:09:01] hug, than codeex. Others might experience it a little bit differently,
[00:09:03] experience it a little bit differently, but it's just something that I've
[00:09:04] but it's just something that I've noticed. Then lastly, Claude is getting
[00:09:06] noticed. Then lastly, Claude is getting more expensive. I heard rumblings online
[00:09:08] more expensive. I heard rumblings online of them thinking about removing or
[00:09:10] of them thinking about removing or planning on removing their $20 a month
[00:09:12] planning on removing their $20 a month plan and making it minimum a little bit
[00:09:14] plan and making it minimum a little bit more expensive. So, just something that
[00:09:15] more expensive. So, just something that you should keep an eye on. Before we
[00:09:17] you should keep an eye on. Before we design, I want to do some setup. Now,
[00:09:19] design, I want to do some setup. Now, this is more foundational. I've covered
[00:09:20] this is more foundational. I've covered it a lot in other videos, so you've if
[00:09:22] it a lot in other videos, so you've if you've already done this, you can skip
[00:09:23] you've already done this, you can skip it, which is connecting Figma MCP and
[00:09:26] it, which is connecting Figma MCP and Figma skills. Now, they are not the same
[00:09:28] Figma skills. Now, they are not the same thing in theory. is Figma MCP is about
[00:09:32] thing in theory. is Figma MCP is about giving AI access to your actual design
[00:09:35] giving AI access to your actual design files, the ability to read the design
[00:09:37] files, the ability to read the design files, understand what's going on. If
[00:09:40] files, understand what's going on. If Figma MCP isn't connected, the AI can't
[00:09:43] Figma MCP isn't connected, the AI can't read anything about your Figma files.
[00:09:45] read anything about your Figma files. But Figma skills are more on teaching
[00:09:47] But Figma skills are more on teaching the AI how to use Figma. If you use
[00:09:50] the AI how to use Figma. If you use Figma MCP, you can still do a lot of the
[00:09:53] Figma MCP, you can still do a lot of the things that you want to do inside Figma.
[00:09:57] things that you want to do inside Figma. um build designs,
[00:10:00] um build designs, but the Figma skills teach the AI how to
[00:10:04] but the Figma skills teach the AI how to apply variables, how to use components,
[00:10:07] apply variables, how to use components, how to better use the Figma canvas. Now,
[00:10:12] how to better use the Figma canvas. Now, luckily for us, we're going to walk
[00:10:13] luckily for us, we're going to walk through how to install Figma MCP and
[00:10:15] through how to install Figma MCP and Figma skills on both Codeex and also in
[00:10:19] Figma skills on both Codeex and also in Claude. Here we are on the Figma
[00:10:21] Claude. Here we are on the Figma community skills site. I'll put a link
[00:10:22] community skills site. I'll put a link for this in the video description. So
[00:10:24] for this in the video description. So these are all the skills that Figma
[00:10:26] these are all the skills that Figma promotes. Now we can create our own
[00:10:28] promotes. Now we can create our own skills for specific purposes unique to
[00:10:30] skills for specific purposes unique to us inside of Claude or Codeex. We'll
[00:10:32] us inside of Claude or Codeex. We'll look at that a little bit later, but
[00:10:34] look at that a little bit later, but there's certain skills that we're going
[00:10:35] there's certain skills that we're going to want to install just for us
[00:10:37] to want to install just for us designers. And it's important to note as
[00:10:38] designers. And it's important to note as well that some of these skills, they're
[00:10:40] well that some of these skills, they're more focused on developers. So they
[00:10:43] more focused on developers. So they don't necessarily apply to us. But we're
[00:10:44] don't necessarily apply to us. But we're going to focus on the ones that apply
[00:10:45] going to focus on the ones that apply most to us today. Right away, we need
[00:10:48] most to us today. Right away, we need this required skill, this Figma use
[00:10:51] this required skill, this Figma use skill. If we don't have this installed,
[00:10:53] skill. If we don't have this installed, the other skills won't work as well. So,
[00:10:55] the other skills won't work as well. So, what we want to do is we want to
[00:10:57] what we want to do is we want to download this from GitHub. And if we hit
[00:10:59] download this from GitHub. And if we hit this MCP server guide, we want to
[00:11:02] this MCP server guide, we want to download the zip entirely. The reason
[00:11:05] download the zip entirely. The reason being is, sorry, if I just go back here,
[00:11:07] being is, sorry, if I just go back here, oops, why did I click that? Sorry. Um,
[00:11:09] oops, why did I click that? Sorry. Um, is on the left hand side, we have all
[00:11:11] is on the left hand side, we have all these skills that go into this larger
[00:11:14] these skills that go into this larger skill. So, we want to install them in
[00:11:16] skill. So, we want to install them in bulk. That said though, there's other
[00:11:18] bulk. That said though, there's other skills that we're interested in that
[00:11:19] skills that we're interested in that aren't bundled in this Figma U skill
[00:11:21] aren't bundled in this Figma U skill because these skills are already bundled
[00:11:23] because these skills are already bundled inside of this. Now, the other ones that
[00:11:26] inside of this. Now, the other ones that we're going to want to apply that I find
[00:11:28] we're going to want to apply that I find a lot of value in are this apply design
[00:11:30] a lot of value in are this apply design system skill. So, say we have existing
[00:11:32] system skill. So, say we have existing Figma designs.
[00:11:34] Figma designs. What we can do is we can call this skill
[00:11:36] What we can do is we can call this skill and connect those designs to our
[00:11:38] and connect those designs to our existing design system. I don't use it a
[00:11:41] existing design system. I don't use it a ton, but I know a lot of designers who
[00:11:43] ton, but I know a lot of designers who have found value in this one right here.
[00:11:45] have found value in this one right here. So, we're going to go ahead and install
[00:11:46] So, we're going to go ahead and install this. Now, for this, what we just want
[00:11:48] this. Now, for this, what we just want here is we just want the skill.md. So,
[00:11:50] here is we just want the skill.md. So, we can come in here and just choose to
[00:11:52] we can come in here and just choose to download that. And probably my favorite
[00:11:54] download that. And probably my favorite skill out of all of these is this audit
[00:11:56] skill out of all of these is this audit design system skill. The output that I
[00:11:58] design system skill. The output that I get from running this is great because
[00:11:59] get from running this is great because what it does is it audits designs that I
[00:12:02] what it does is it audits designs that I already have and checks to see where
[00:12:05] already have and checks to see where that design system is not applied, where
[00:12:07] that design system is not applied, where it's applying the wrong component, the
[00:12:08] it's applying the wrong component, the wrong variable, the wrong style, where
[00:12:10] wrong variable, the wrong style, where those variable styles or components
[00:12:12] those variable styles or components aren't being used at all. and then it
[00:12:14] aren't being used at all. and then it will fix it based on that. It's my
[00:12:16] will fix it based on that. It's my favorite skill that they offer. So
[00:12:17] favorite skill that they offer. So again, it's just the skill.md that we're
[00:12:19] again, it's just the skill.md that we're really interested in here. Let's go
[00:12:20] really interested in here. Let's go ahead and download that and let's open
[00:12:21] ahead and download that and let's open up Claude. Here we are in Claude. And in
[00:12:25] up Claude. Here we are in Claude. And in Claude on the lefth hand side, you're
[00:12:26] Claude on the lefth hand side, you're going to see like a customize button. Uh
[00:12:28] going to see like a customize button. Uh so just hit customize. You're going to
[00:12:30] so just hit customize. You're going to navigate to a page like this. So the way
[00:12:33] navigate to a page like this. So the way in which we install things is different
[00:12:35] in which we install things is different on cloud than it is codecs.
[00:12:37] on cloud than it is codecs. Let's start off by installing our Figma
[00:12:41] Let's start off by installing our Figma U skills. So I want to show you
[00:12:42] U skills. So I want to show you something here. It's where a lot of
[00:12:44] something here. It's where a lot of designers tend to get tripped up is we
[00:12:46] designers tend to get tripped up is we want to bulk upload that zip file
[00:12:49] want to bulk upload that zip file because it contains all of those
[00:12:50] because it contains all of those subsklls. So let's choose to uh go ahead
[00:12:54] subsklls. So let's choose to uh go ahead and upload a plugin and let's just drag
[00:12:56] and upload a plugin and let's just drag in that first zip file that we had
[00:12:58] in that first zip file that we had downloaded. So the MCP server guide zip
[00:13:01] downloaded. So the MCP server guide zip file. So let's go ahead upload that. So
[00:13:04] file. So let's go ahead upload that. So I can see the plugin is installed and
[00:13:06] I can see the plugin is installed and ready to use. So, if we click in here,
[00:13:08] ready to use. So, if we click in here, you can see that the plugin that
[00:13:09] you can see that the plugin that includes the Figma MCP server and skills
[00:13:12] includes the Figma MCP server and skills for common workflows.
[00:13:15] for common workflows. This is where it's not super clear
[00:13:18] This is where it's not super clear because we have all of those skills,
[00:13:22] because we have all of those skills, but under connectors,
[00:13:24] but under connectors, we're technically not connected to Figma
[00:13:26] we're technically not connected to Figma yet. So, despite the fact that it says
[00:13:28] yet. So, despite the fact that it says includes the Figma MCP servers, we still
[00:13:31] includes the Figma MCP servers, we still need to initiate that connection. So
[00:13:33] need to initiate that connection. So under connectors, let's just choose to
[00:13:35] under connectors, let's just choose to install and then just choose to connect.
[00:13:38] install and then just choose to connect. And what that's going to do is going to
[00:13:40] And what that's going to do is going to open up Figma in the browser. So just
[00:13:44] open up Figma in the browser. So just sign into Figma there uh and go ahead
[00:13:46] sign into Figma there uh and go ahead and connect. Sorry, I'm trying to do it
[00:13:48] and connect. Sorry, I'm trying to do it offline here, but it's being a little
[00:13:49] offline here, but it's being a little bit slow. So I'm just going to pause the
[00:13:50] bit slow. So I'm just going to pause the video here. It finally loaded inside of
[00:13:52] video here. It finally loaded inside of my browser. So I just hit agree and
[00:13:54] my browser. So I just hit agree and allow access. And I can see now now that
[00:13:57] allow access. And I can see now now that Figma MCP is installed. So, what's
[00:14:00] Figma MCP is installed. So, what's important to note here just by uploading
[00:14:02] important to note here just by uploading uh that zip file, even though it says
[00:14:05] uh that zip file, even though it says includes the Figma MCP servers, you
[00:14:07] includes the Figma MCP servers, you still need to connect it. The
[00:14:09] still need to connect it. The alternative way that you can do it is if
[00:14:10] alternative way that you can do it is if you don't want the Figma skills is just
[00:14:12] you don't want the Figma skills is just under connectors, you can choose to
[00:14:13] under connectors, you can choose to browse connectors and then just search
[00:14:15] browse connectors and then just search Figma. And again, it's this one right
[00:14:17] Figma. And again, it's this one right here uh that we're interested in and we
[00:14:19] here uh that we're interested in and we have already installed it. So, all is
[00:14:21] have already installed it. So, all is well. Beautiful. Now, those are
[00:14:24] well. Beautiful. Now, those are installed. Here we are inside of codeex.
[00:14:28] installed. Here we are inside of codeex. This is where things can kind of get a
[00:14:30] This is where things can kind of get a little bit weird because people
[00:14:32] little bit weird because people experience things differently and I
[00:14:34] experience things differently and I don't know why.
[00:14:37] don't know why. Like as of yesterday,
[00:14:39] Like as of yesterday, I was able to have this Figma plugin
[00:14:42] I was able to have this Figma plugin installed which consists of all these
[00:14:44] installed which consists of all these different skills. I just chatted with
[00:14:46] different skills. I just chatted with someone who was on a personal plan just
[00:14:49] someone who was on a personal plan just like me and they're able to install the
[00:14:53] like me and they're able to install the Figma plugin. So you might there's
[00:14:55] Figma plugin. So you might there's there's I don't know what's going on.
[00:14:58] there's I don't know what's going on. put it that way. But the Figma plugin
[00:15:00] put it that way. But the Figma plugin here actually has all of the skills that
[00:15:03] here actually has all of the skills that we had installed in that Figma use
[00:15:06] we had installed in that Figma use skill. So, we didn't need to download
[00:15:09] skill. So, we didn't need to download the item from GitHub, upload it. It's
[00:15:11] the item from GitHub, upload it. It's all wrapped inside of this one skill.
[00:15:15] all wrapped inside of this one skill. I'm hearing mumblings where this is only
[00:15:17] I'm hearing mumblings where this is only going to be available to workspace
[00:15:18] going to be available to workspace users. Maybe that's just slowly getting
[00:15:20] users. Maybe that's just slowly getting rolled out. I don't know. But there's a
[00:15:22] rolled out. I don't know. But there's a workaround here where under skills again
[00:15:25] workaround here where under skills again just at the top here. And if you search
[00:15:27] just at the top here. And if you search Figma, you just go ahead and install all
[00:15:30] Figma, you just go ahead and install all of those same skills. So starting off
[00:15:32] of those same skills. So starting off with that Figma MCP uh that Figma code
[00:15:36] with that Figma MCP uh that Figma code connect Figma create design systems
[00:15:37] connect Figma create design systems rules. So all of those subs skills that
[00:15:40] rules. So all of those subs skills that were in that zip file that we had
[00:15:42] were in that zip file that we had installed a little bit earlier. They're
[00:15:45] installed a little bit earlier. They're now installed for us and ready to rock.
[00:15:48] now installed for us and ready to rock. So a little bit of a different workflow.
[00:15:50] So a little bit of a different workflow. You know, I'm seeing different thing
[00:15:53] You know, I'm seeing different thing people different people experiencing
[00:15:54] people different people experiencing different things. Some skills are
[00:15:55] different things. Some skills are available, some skills are not
[00:15:57] available, some skills are not available. Sometimes that plugin is
[00:15:59] available. Sometimes that plugin is available, sometimes that plugin is not
[00:16:00] available, sometimes that plugin is not available for users who are just on a
[00:16:02] available for users who are just on a personal plan like myself. Just
[00:16:04] personal plan like myself. Just something to be aware of sometimes. Um,
[00:16:08] something to be aware of sometimes. Um, yeah, I know it can be confusing. One
[00:16:09] yeah, I know it can be confusing. One thing real quick, Codex finally got his
[00:16:11] thing real quick, Codex finally got his act together and allowed me to install
[00:16:13] act together and allowed me to install this Figma plugin. So, I had installed
[00:16:15] this Figma plugin. So, I had installed it. In terms of basic setup, that's kind
[00:16:17] it. In terms of basic setup, that's kind of it. We have those, you know, Claude,
[00:16:19] of it. We have those, you know, Claude, Codeex, whatever installed. We're using
[00:16:21] Codeex, whatever installed. We're using it. We're signed up. We have Figma MCP
[00:16:23] it. We're signed up. We have Figma MCP connected. We have those Figma Figma
[00:16:25] connected. We have those Figma Figma skills also installed. Let's go through
[00:16:27] skills also installed. Let's go through the tools now, how they work, how to
[00:16:30] the tools now, how they work, how to make the most of them. I want to start
[00:16:31] make the most of them. I want to start off with Google Stitch for a couple
[00:16:34] off with Google Stitch for a couple reasons. One, because Google Stitch, we
[00:16:38] reasons. One, because Google Stitch, we can't train it on our on our design
[00:16:40] can't train it on our on our design system the way that we would expect. We
[00:16:42] system the way that we would expect. We can't paste in a Figma file here and
[00:16:45] can't paste in a Figma file here and build skills around our design system.
[00:16:48] build skills around our design system. That's a big limitation to Stitch as of
[00:16:50] That's a big limitation to Stitch as of right now. I'm sure they're going to
[00:16:52] right now. I'm sure they're going to change it in the future, but as of right
[00:16:53] change it in the future, but as of right now, that is not existing functionality.
[00:16:56] now, that is not existing functionality. The other reason why I want to start
[00:16:57] The other reason why I want to start with Google Stitch is because it's most
[00:17:00] with Google Stitch is because it's most different. It's way different than claw
[00:17:03] different. It's way different than claw design. It's way different than
[00:17:05] design. It's way different than generating designs with clawed code or
[00:17:08] generating designs with clawed code or codecs.
[00:17:09] codecs. I want to show you what I mean. One
[00:17:12] I want to show you what I mean. One thing I want to do here is we're just
[00:17:13] thing I want to do here is we're just going to start off with thinking with
[00:17:14] going to start off with thinking with 3.1 Pro because it's not going to be as
[00:17:17] 3.1 Pro because it's not going to be as quick, but it's get better quality with
[00:17:19] quick, but it's get better quality with it. So, we're going to start with
[00:17:21] it. So, we're going to start with thinking with 3.1 Pro and let's build a
[00:17:25] thinking with 3.1 Pro and let's build a financial app because my background is
[00:17:27] financial app because my background is in financial technology. So, what
[00:17:29] in financial technology. So, what desktop web experience should we start
[00:17:31] desktop web experience should we start with? And one thing I want to call out
[00:17:33] with? And one thing I want to call out here is Google Stitch's web designs are
[00:17:37] here is Google Stitch's web designs are never really that good. The app designs
[00:17:40] never really that good. The app designs usually way better.
[00:17:42] usually way better. What I want to do is let's just say run
[00:17:45] What I want to do is let's just say run it again. My background is a financial
[00:17:46] it again. My background is a financial technology. So, uh, build a financial,
[00:17:52] technology. So, uh, build a financial, uh, management app for financial
[00:17:56] uh, management app for financial advisors. Let's just run that. This
[00:17:59] advisors. Let's just run that. This here, great result. And this took about
[00:18:01] here, great result. And this took about 30 seconds to generate.
[00:18:05] 30 seconds to generate. So, pretty quick. Now, there's still
[00:18:07] So, pretty quick. Now, there's still issues with it. It's not like something
[00:18:09] issues with it. It's not like something I can take right away and be like,
[00:18:11] I can take right away and be like, "Yeah, sending this to developer, done."
[00:18:14] "Yeah, sending this to developer, done." But this is really good for iteration.
[00:18:17] But this is really good for iteration. I need to get some layout inspiration
[00:18:19] I need to get some layout inspiration fast based on an app that I'm trying to
[00:18:22] fast based on an app that I'm trying to build. Of course, beef up the prompt a
[00:18:23] build. Of course, beef up the prompt a little bit, but this allows me to sort
[00:18:26] little bit, but this allows me to sort of get an idea of the structure down,
[00:18:28] of get an idea of the structure down, the types of data that I should display,
[00:18:31] the types of data that I should display, how I should structure my bottom nav.
[00:18:34] how I should structure my bottom nav. And again, some of this stuff looks AI
[00:18:36] And again, some of this stuff looks AI generated. This is clearly an AI
[00:18:38] generated. This is clearly an AI generated widget, but that's okay. It's
[00:18:41] generated widget, but that's okay. It's going to get better. I can see. So we
[00:18:43] going to get better. I can see. So we have our client list, our client
[00:18:45] have our client list, our client profile, advisor dashboard, rebalancing
[00:18:47] profile, advisor dashboard, rebalancing tool.
[00:18:49] tool. This gives me enough to get started with
[00:18:51] This gives me enough to get started with and start having meaningful
[00:18:52] and start having meaningful conversations with others on my team
[00:18:54] conversations with others on my team around what it is we're going going to
[00:18:57] around what it is we're going going to want to display on each page. I want to
[00:18:59] want to display on each page. I want to show you something here is I ran the
[00:19:00] show you something here is I ran the exact same prompt but chose for a
[00:19:02] exact same prompt but chose for a desktop and this is what it gave me. The
[00:19:05] desktop and this is what it gave me. The result is way worse. Way worse. So
[00:19:09] result is way worse. Way worse. So whenever it comes to working with Google
[00:19:11] whenever it comes to working with Google Stitch, you are going to get way better
[00:19:13] Stitch, you are going to get way better results working with mobile designs than
[00:19:16] results working with mobile designs than you are on desktop. Like this here,
[00:19:19] you are on desktop. Like this here, Super Junior feels super AI generated.
[00:19:22] Super Junior feels super AI generated. Not great. But that's not to say that
[00:19:24] Not great. But that's not to say that Stitch can't give you good results. A
[00:19:26] Stitch can't give you good results. A lot of times the base prompts that it
[00:19:28] lot of times the base prompts that it might give you as examples will give you
[00:19:30] might give you as examples will give you really good results. And sometimes even
[00:19:32] really good results. And sometimes even the prompts that you run give you
[00:19:34] the prompts that you run give you results that look like this where it's
[00:19:37] results that look like this where it's clean. It looks good. It has polish to
[00:19:42] clean. It looks good. It has polish to it.
[00:19:44] it. So stitches because it's still in beta.
[00:19:48] So stitches because it's still in beta. You never get a accurate
[00:19:52] You never get a accurate result all the time. Sometimes it's hit
[00:19:55] result all the time. Sometimes it's hit or miss. Sometimes the result is not
[00:19:58] or miss. Sometimes the result is not good at all. Going back to the mobile
[00:20:00] good at all. Going back to the mobile version though, is Stitch is really good
[00:20:02] version though, is Stitch is really good for just basic iteration like this
[00:20:05] for just basic iteration like this market overview page. this advisor
[00:20:08] market overview page. this advisor dashboard. Maybe I want additional
[00:20:09] dashboard. Maybe I want additional variations. I can come in here under
[00:20:11] variations. I can come in here under generate, select variations, select the
[00:20:14] generate, select variations, select the number of variations I want, specify a
[00:20:17] number of variations I want, specify a creative range like more refining,
[00:20:19] creative range like more refining, exploring, or reimagining completely.
[00:20:20] exploring, or reimagining completely. So, let's just explore some different
[00:20:23] So, let's just explore some different versions here. Choose what it is we want
[00:20:25] versions here. Choose what it is we want to vary. So, maybe just layouts. And now
[00:20:27] to vary. So, maybe just layouts. And now we can choose to generate different
[00:20:29] we can choose to generate different variations based on specific screens,
[00:20:31] variations based on specific screens, specific elements. We're off to the
[00:20:33] specific elements. We're off to the right here. What it's going to do, it's
[00:20:35] right here. What it's going to do, it's going to generate those variations.
[00:20:36] going to generate those variations. There we go. You can see that they're
[00:20:38] There we go. You can see that they're they're generating here.
[00:20:40] they're generating here. Stitch glitched out while it was
[00:20:42] Stitch glitched out while it was generating those two designs. So, I just
[00:20:43] generating those two designs. So, I just had to generate it again this time of
[00:20:45] had to generate it again this time of three. And this is what it came back
[00:20:45] three. And this is what it came back with. So, just some different layouts
[00:20:47] with. So, just some different layouts here where this one's got that bar graph
[00:20:50] here where this one's got that bar graph again. Sure, some of the bars are hard
[00:20:51] again. Sure, some of the bars are hard to see here. You know, active client
[00:20:53] to see here. You know, active client pulse, priority alerts, whereas this
[00:20:55] pulse, priority alerts, whereas this version is a little bit different where
[00:20:56] version is a little bit different where you have those big metric cards right at
[00:20:58] you have those big metric cards right at the top. Then you move into an
[00:20:59] the top. Then you move into an itinerary, what they need to focus on
[00:21:00] itinerary, what they need to focus on today. And this one is even a little bit
[00:21:02] today. And this one is even a little bit more different where you have that AUM
[00:21:04] more different where you have that AUM like their assets under management like
[00:21:06] like their assets under management like a big and bold at the top with other
[00:21:08] a big and bold at the top with other metrics. Then you get into your growth.
[00:21:12] metrics. Then you get into your growth. But as you can see like these aren't
[00:21:14] But as you can see like these aren't production level designs. I wouldn't
[00:21:17] production level designs. I wouldn't take these and show it to a client. I
[00:21:20] take these and show it to a client. I wouldn't take these and say developer
[00:21:22] wouldn't take these and say developer build this out for me. But where is it
[00:21:25] build this out for me. But where is it I'm using Google Stitch right now? Where
[00:21:27] I'm using Google Stitch right now? Where is it? I'm using Stitch for fast
[00:21:30] is it? I'm using Stitch for fast generation for very very early stage
[00:21:32] generation for very very early stage concepts without burning tokens. I'm not
[00:21:35] concepts without burning tokens. I'm not looking for polished designs. I'm
[00:21:36] looking for polished designs. I'm looking for types of widgets, types of
[00:21:38] looking for types of widgets, types of data that we might want to display to
[00:21:40] data that we might want to display to sort of have some of these more early
[00:21:42] sort of have some of these more early conversations on what's important with
[00:21:45] conversations on what's important with internal stakeholders. I'm only going to
[00:21:47] internal stakeholders. I'm only going to bring Claude and Codeex and Claw Design
[00:21:49] bring Claude and Codeex and Claw Design into the equation once I know absolutely
[00:21:52] into the equation once I know absolutely for sure. So, I got a couple options
[00:21:54] for sure. So, I got a couple options here. Either I can burn a ton of tokens
[00:21:57] here. Either I can burn a ton of tokens in a ton of time generating these
[00:21:59] in a ton of time generating these polished first drafts
[00:22:02] polished first drafts inside of Claude and Codeex or I can use
[00:22:05] inside of Claude and Codeex or I can use Stitch because it's basically free to
[00:22:09] Stitch because it's basically free to spin up a bunch of different variations
[00:22:11] spin up a bunch of different variations of things of ways data can be displayed
[00:22:14] of things of ways data can be displayed and data points we want to show and
[00:22:16] and data points we want to show and types of widgets to chat with again just
[00:22:19] types of widgets to chat with again just internal stakeholders. I wouldn't show
[00:22:21] internal stakeholders. I wouldn't show this to a client or anyone external
[00:22:24] this to a client or anyone external on what really speaks to them, the types
[00:22:26] on what really speaks to them, the types of data that they like, how they like
[00:22:28] of data that they like, how they like data to be displayed because it's going
[00:22:30] data to be displayed because it's going to better inform what I'm either able to
[00:22:33] to better inform what I'm either able to one then reproduce in Figma or two build
[00:22:37] one then reproduce in Figma or two build out in clawed code and codeex
[00:22:42] out in clawed code and codeex without having to go through the level
[00:22:44] without having to go through the level of variation we did in Stitch, which is
[00:22:46] of variation we did in Stitch, which is going to burn a million and two tokens
[00:22:49] going to burn a million and two tokens inside of the other tools. And hopefully
[00:22:51] inside of the other tools. And hopefully you can see that Stitch is designer
[00:22:53] you can see that Stitch is designer friendly. You know, we can move things
[00:22:55] friendly. You know, we can move things around. We can preview things. We can
[00:22:57] around. We can preview things. We can choose to mod modify things. We can, you
[00:22:59] choose to mod modify things. We can, you know, generate those variations and
[00:23:01] know, generate those variations and regenerate designs. We can generate a
[00:23:03] regenerate designs. We can generate a desktop web version based on that. And
[00:23:05] desktop web version based on that. And we can even tweak like the design system
[00:23:08] we can even tweak like the design system uh here too. We can adjust the color
[00:23:10] uh here too. We can adjust the color palette.
[00:23:11] palette. But again, it doesn't produce those
[00:23:13] But again, it doesn't produce those production level designs that I had just
[00:23:15] production level designs that I had just talked about. But what's the other key
[00:23:19] talked about. But what's the other key tool in the AI space that's really
[00:23:21] tool in the AI space that's really tailored to designers? That's claude
[00:23:24] tailored to designers? That's claude design. It's going to be a nice segue
[00:23:26] design. It's going to be a nice segue into sort of the claude codec space.
[00:23:29] into sort of the claude codec space. Here we are in claw design. And I want
[00:23:32] Here we are in claw design. And I want to because claude design stitch, they're
[00:23:34] to because claude design stitch, they're the design focused
[00:23:38] the design focused AI tools, the real popular ones that are
[00:23:40] AI tools, the real popular ones that are out there. And I want to showcase the
[00:23:43] out there. And I want to showcase the different quality that you get on claw
[00:23:46] different quality that you get on claw design versus what you get on stitch. So
[00:23:50] design versus what you get on stitch. So here we are in claw design. I'll put a
[00:23:52] here we are in claw design. I'll put a link for this the video description.
[00:23:53] link for this the video description. Let's just create a new project and not
[00:23:55] Let's just create a new project and not select any particular design system.
[00:23:56] select any particular design system. Just call it high fidelity. Just make it
[00:23:59] Just call it high fidelity. Just make it a high fidelity, not a wireframe. Sorry.
[00:24:01] a high fidelity, not a wireframe. Sorry. Um so let's just call it uh AI demo.
[00:24:05] Um so let's just call it uh AI demo. Let's create this and let's go ahead and
[00:24:09] Let's create this and let's go ahead and build a prompt. Now, let's run the exact
[00:24:11] build a prompt. Now, let's run the exact same prompt as we did in Google Stitch.
[00:24:14] same prompt as we did in Google Stitch. Build me oh, forgot my please. Please
[00:24:16] Build me oh, forgot my please. Please build me
[00:24:18] build me a financial management app for financial
[00:24:23] a financial management app for financial advisors. Oh, yeah. Not the best prompt
[00:24:25] advisors. Oh, yeah. Not the best prompt in the world. Uh, not a really good
[00:24:27] in the world. Uh, not a really good prompt at all, but still the same. Let's
[00:24:29] prompt at all, but still the same. Let's run. And we get some questions. And this
[00:24:31] run. And we get some questions. And this is nice because with Google Stitch just
[00:24:33] is nice because with Google Stitch just like basically just went ahead and did
[00:24:34] like basically just went ahead and did things, but Claw Design wants to know a
[00:24:38] things, but Claw Design wants to know a little bit more detail. And it's going
[00:24:40] little bit more detail. And it's going to help us produce a more polished
[00:24:42] to help us produce a more polished version the very first time. So who's
[00:24:46] version the very first time. So who's the primary user? Let's go uh financial
[00:24:49] the primary user? Let's go uh financial planner. Uh which workflows matter most?
[00:24:53] planner. Uh which workflows matter most? So I can select let's go performance
[00:24:56] So I can select let's go performance reporting. Uh select a couple of these
[00:24:59] reporting. Uh select a couple of these and portfolio dashboard. Which screen
[00:25:02] and portfolio dashboard. Which screen should be the hero? Uh advisor home
[00:25:05] should be the hero? Uh advisor home platform. Let's just go desktop.
[00:25:08] platform. Let's just go desktop. Let's go aesthetic direction. Have it
[00:25:11] Let's go aesthetic direction. Have it decide for me. Information density
[00:25:13] decide for me. Information density again. Decide for me. All these other
[00:25:15] again. Decide for me. All these other ones, let's just select decide for me.
[00:25:17] ones, let's just select decide for me. But you can see the level of detail that
[00:25:19] But you can see the level of detail that it's asking us to try to get the best
[00:25:21] it's asking us to try to get the best result the very first time. So decide
[00:25:24] result the very first time. So decide for me and then decide for me. Let's run
[00:25:28] for me and then decide for me. Let's run this. You can see that it came up with a
[00:25:30] this. You can see that it came up with a list of todos, but I'm already on minute
[00:25:32] list of todos, but I'm already on minute six. It doesn't look like any of these
[00:25:34] six. It doesn't look like any of these are actually built yet. So you can see
[00:25:37] are actually built yet. So you can see how already how with stitch the results
[00:25:38] how already how with stitch the results came back after like 15 seconds not even
[00:25:40] came back after like 15 seconds not even sometimes. But with claw design there's
[00:25:44] sometimes. But with claw design there's a lot of waiting involved which if we're
[00:25:47] a lot of waiting involved which if we're looking for fast variations and
[00:25:48] looking for fast variations and iterations just exploring certain
[00:25:51] iterations just exploring certain concepts what data could go where what
[00:25:55] concepts what data could go where what data might resonate with an internal
[00:25:57] data might resonate with an internal stakeholder.
[00:25:59] stakeholder. Cloud design really slows down a lot of
[00:26:02] Cloud design really slows down a lot of that fast iteration we want to do in
[00:26:04] that fast iteration we want to do in those early stages. I know what you're
[00:26:06] those early stages. I know what you're thinking. But Kirk, I have all the time
[00:26:08] thinking. But Kirk, I have all the time in the world. So if claw design gives me
[00:26:10] in the world. So if claw design gives me a better result first try to share with
[00:26:13] a better result first try to share with stakeholders, I'm only going to use claw
[00:26:15] stakeholders, I'm only going to use claw design. Let's pause for a sec because
[00:26:18] design. Let's pause for a sec because the usage of claw design is limited in
[00:26:21] the usage of claw design is limited in this early stage and it is going to get
[00:26:23] this early stage and it is going to get expensive. Claude code is an expensive
[00:26:26] expensive. Claude code is an expensive tool. I believe I've said it before,
[00:26:28] tool. I believe I've said it before, I'll say it again. I believe it was
[00:26:29] I'll say it again. I believe it was Uber. they ran out of their AI
[00:26:31] Uber. they ran out of their AI development budget in like 3 to four
[00:26:33] development budget in like 3 to four months that they had set for the year.
[00:26:36] months that they had set for the year. Now unless you're working at a company
[00:26:38] Now unless you're working at a company like Apple or Google where your budget I
[00:26:40] like Apple or Google where your budget I don't want to say is infinite then
[00:26:43] don't want to say is infinite then chances are the way in which you can
[00:26:45] chances are the way in which you can leverage a tool like claw design once
[00:26:47] leverage a tool like claw design once they pile on the features pile on like
[00:26:49] they pile on the features pile on like included in all the plans and everything
[00:26:50] included in all the plans and everything like that and start charging for it a
[00:26:52] like that and start charging for it a lot more than they are now the cost is
[00:26:55] lot more than they are now the cost is going to add up. So, you need to be
[00:26:57] going to add up. So, you need to be really strategic around where you're
[00:26:59] really strategic around where you're going to be using your tokens. And the
[00:27:02] going to be using your tokens. And the result here
[00:27:04] result here is way better. Look at this. This is
[00:27:06] is way better. Look at this. This is like
[00:27:08] like senior level design. It looks clean. The
[00:27:12] senior level design. It looks clean. The font is nice. The different font
[00:27:13] font is nice. The different font treatments here. How this is like normal
[00:27:16] treatments here. How this is like normal font, not indented. This is slightly
[00:27:18] font, not indented. This is slightly different color. It's indented and or
[00:27:21] different color. It's indented and or not indented, sorry, italicized. Um, we
[00:27:24] not indented, sorry, italicized. Um, we have this interactive graph. We have
[00:27:26] have this interactive graph. We have this allocation. We have this book of
[00:27:29] this allocation. We have this book of business. We have today's agenda. It's
[00:27:30] business. We have today's agenda. It's all interactive or at least some of it
[00:27:32] all interactive or at least some of it is. This is great. You know, like you
[00:27:36] is. This is great. You know, like you can show this to a client and be like,
[00:27:38] can show this to a client and be like, we spun this up
[00:27:40] we spun this up and they wouldn't bat an eye. Whereas
[00:27:43] and they wouldn't bat an eye. Whereas with the Stitch example, they would bat
[00:27:45] with the Stitch example, they would bat an eye. Despite the fact how good this
[00:27:47] an eye. Despite the fact how good this looks, there's an issue is it only built
[00:27:50] looks, there's an issue is it only built the dashboard. It didn't build any of
[00:27:53] the dashboard. It didn't build any of the other pages. Now, you might be
[00:27:56] the other pages. Now, you might be thinking, let's just have it build those
[00:27:57] thinking, let's just have it build those pages now. But today is currently
[00:27:59] pages now. But today is currently Wednesday. You can see that my usage
[00:28:01] Wednesday. You can see that my usage resets in Claw Design on Tuesday. I did
[00:28:04] resets in Claw Design on Tuesday. I did not use Claw Design yesterday. So, that
[00:28:06] not use Claw Design yesterday. So, that one dashboard took me 8% through my
[00:28:09] one dashboard took me 8% through my usage.
[00:28:11] usage. So, you can see how easy it is to use up
[00:28:16] So, you can see how easy it is to use up what you're available to use in claw
[00:28:19] what you're available to use in claw design. So if we're inefficient in the
[00:28:21] design. So if we're inefficient in the way in which we
[00:28:24] way in which we start just start with claw design for a
[00:28:26] start just start with claw design for a lot of that iteration by the time we get
[00:28:28] lot of that iteration by the time we get feedback from our stakeholders all of a
[00:28:31] feedback from our stakeholders all of a sudden we have to wait in order to
[00:28:33] sudden we have to wait in order to reprompt inside claw design. So what
[00:28:35] reprompt inside claw design. So what you're doing is you're using stitch to
[00:28:37] you're doing is you're using stitch to inform your prompt for claw design
[00:28:41] inform your prompt for claw design because with Google stitch what's your
[00:28:42] because with Google stitch what's your objective midfi wireframing as we saw
[00:28:44] objective midfi wireframing as we saw it's not hi-fi it's not lowfi with just
[00:28:46] it's not hi-fi it's not lowfi with just basic shapes it's somewhere in between
[00:28:48] basic shapes it's somewhere in between to align on metrics align on potential
[00:28:50] to align on metrics align on potential formats align on charts and then when
[00:28:53] formats align on charts and then when you move into claw design which is more
[00:28:55] you move into claw design which is more of that super hi-fi wireframing it's hey
[00:28:58] of that super hi-fi wireframing it's hey claude here's the specific data that we
[00:29:00] claude here's the specific data that we want on the dashboard here's the format
[00:29:02] want on the dashboard here's the format that this should be in where we want our
[00:29:04] that this should be in where we want our metrics along the top in four cards.
[00:29:06] metrics along the top in four cards. These are the specific metrics because I
[00:29:07] These are the specific metrics because I aligned on these with my stakeholder
[00:29:09] aligned on these with my stakeholder after reviewing what Google Stitch gave
[00:29:10] after reviewing what Google Stitch gave us. Below that, we are going to have a
[00:29:13] us. Below that, we are going to have a performance chart and it's going to be a
[00:29:14] performance chart and it's going to be a line chart, not a bar chart. And here is
[00:29:17] line chart, not a bar chart. And here is why. Ultimately, your goal is just to
[00:29:19] why. Ultimately, your goal is just to reduce the number of edits you need in
[00:29:20] reduce the number of edits you need in cloud design. The closer you can get to
[00:29:24] cloud design. The closer you can get to it first try in claw design, the better.
[00:29:27] it first try in claw design, the better. But you can't do it if you haven't had
[00:29:29] But you can't do it if you haven't had those good convos with internal
[00:29:31] those good convos with internal stakeholders around what they like the
[00:29:33] stakeholders around what they like the data, what data should be displayed
[00:29:34] data, what data should be displayed where. And that's where Google Stitch
[00:29:37] where. And that's where Google Stitch comes in. Having better early stage
[00:29:40] comes in. Having better early stage convos at a much faster rate. Another
[00:29:43] convos at a much faster rate. Another thing that's not great about claw
[00:29:46] thing that's not great about claw design, despite the fact it's not very
[00:29:48] design, despite the fact it's not very efficient, I guess, is it doesn't feel
[00:29:51] efficient, I guess, is it doesn't feel like a design tool. So, I'm going to
[00:29:54] like a design tool. So, I'm going to have it actually build another
[00:29:55] have it actually build another variation. Build
[00:29:58] variation. Build me another
[00:30:01] me another variation. Or actually, let's say uh
[00:30:04] variation. Or actually, let's say uh build the book page in uh instead. Build
[00:30:07] build the book page in uh instead. Build the book page instead. Uh then lay out
[00:30:11] the book page instead. Uh then lay out the designs
[00:30:14] the designs side by side
[00:30:16] side by side so I can view them. So, build the book
[00:30:19] so I can view them. So, build the book page instead. So referring to this page
[00:30:21] page instead. So referring to this page right here because we only really have
[00:30:22] right here because we only really have our dashboard. Uh then lay out the
[00:30:25] our dashboard. Uh then lay out the designs side by side so I can view them.
[00:30:27] designs side by side so I can view them. Let's run. And now this is what it came
[00:30:28] Let's run. And now this is what it came back with. So now we have that book of
[00:30:30] back with. So now we have that book of business. So we have that book page and
[00:30:32] business. So we have that book page and again looks pretty good. We have our
[00:30:34] again looks pretty good. We have our filters here on the side. We have these
[00:30:36] filters here on the side. We have these this great metrics treatment at the top.
[00:30:38] this great metrics treatment at the top. We have a list of everyone who's in like
[00:30:40] We have a list of everyone who's in like our financial advisor book. I think it
[00:30:42] our financial advisor book. I think it looks pretty good. And now what's nice
[00:30:44] looks pretty good. And now what's nice is because I told it I want to view
[00:30:45] is because I told it I want to view things side by side. We can kind of view
[00:30:47] things side by side. We can kind of view it like a canvas. But what's the issue
[00:30:49] it like a canvas. But what's the issue here? I can't really like,
[00:30:52] here? I can't really like, you know, I can move things around, but
[00:30:55] you know, I can move things around, but I can't modify. Oh, look, these are
[00:30:58] I can't modify. Oh, look, these are these are interactive, too. That's nice.
[00:31:00] these are interactive, too. That's nice. Built those out right away, but I can't
[00:31:02] Built those out right away, but I can't move things side to side. Like, I can
[00:31:05] move things side to side. Like, I can like that scroll effect works. We got to
[00:31:08] like that scroll effect works. We got to ah, what's going on there? Let me delete
[00:31:10] ah, what's going on there? Let me delete that. Um, but I can't rearrange things
[00:31:13] that. Um, but I can't rearrange things like I would on Figma. It's very, very
[00:31:15] like I would on Figma. It's very, very static. And sorry, it's still at running
[00:31:17] static. And sorry, it's still at running and changing things as I go. And if I
[00:31:19] and changing things as I go. And if I wanted to make tweaks here, what I would
[00:31:21] wanted to make tweaks here, what I would need to do is I can just choose to
[00:31:23] need to do is I can just choose to select items and edit it or I would have
[00:31:25] select items and edit it or I would have to leave comments about change this,
[00:31:27] to leave comments about change this, change this, just leave that as a
[00:31:29] change this, just leave that as a comment, you know, um different
[00:31:31] comment, you know, um different treatment, different treatments, and
[00:31:33] treatment, different treatments, and things like that. It's all very Claude
[00:31:36] things like that. It's all very Claude has to do it manually. It's not
[00:31:38] has to do it manually. It's not something that we can just move around
[00:31:40] something that we can just move around or make those tweaks on the fly,
[00:31:42] or make those tweaks on the fly, especially for those larger layout
[00:31:44] especially for those larger layout changes. And now after that second
[00:31:47] changes. And now after that second prompt, we're now at 15% used of our
[00:31:51] prompt, we're now at 15% used of our claw design usage, which resets next
[00:31:54] claw design usage, which resets next week. So you can imagine how frustrating
[00:31:56] week. So you can imagine how frustrating it would be if you ran out of claw
[00:32:00] it would be if you ran out of claw design tokens when you're in the middle
[00:32:02] design tokens when you're in the middle of like a design sprint. Not fun. One
[00:32:05] of like a design sprint. Not fun. One thing I did forget to call out here is
[00:32:07] thing I did forget to call out here is there is the ability to hand this off to
[00:32:10] there is the ability to hand this off to Claude Code. We're going to get to
[00:32:11] Claude Code. We're going to get to Claude Code in just a second. We're not
[00:32:13] Claude Code in just a second. We're not going to use the designs here. We move
[00:32:14] going to use the designs here. We move to claw code. But something I did just
[00:32:17] to claw code. But something I did just want to call out that I forgot to
[00:32:18] want to call out that I forgot to highlight here. Everything else within
[00:32:20] highlight here. Everything else within claw design. Relatively straightforward
[00:32:22] claw design. Relatively straightforward stuff. There's nothing out of the
[00:32:24] stuff. There's nothing out of the ordinary that would stop you from using
[00:32:26] ordinary that would stop you from using this effectively. In order to hand off
[00:32:28] this effectively. In order to hand off to claude code, just hit hand off to
[00:32:29] to claude code, just hit hand off to claude code. Copy this command and then
[00:32:31] claude code. Copy this command and then go inside to claude code and just paste
[00:32:34] go inside to claude code and just paste that in a new chat. We're now familiar
[00:32:36] that in a new chat. We're now familiar with stitch and claw design limitations
[00:32:38] with stitch and claw design limitations and where they're used generally used. I
[00:32:41] and where they're used generally used. I want to show you the different outputs
[00:32:43] want to show you the different outputs with claude code and codeex then. So
[00:32:45] with claude code and codeex then. So remove claw design and like the design
[00:32:48] remove claw design and like the design more design focused tools and just
[00:32:50] more design focused tools and just inside claude code and codec. Let's run
[00:32:52] inside claude code and codec. Let's run the exact same prompt we've been working
[00:32:54] the exact same prompt we've been working with. It's not just about comparing
[00:32:55] with. It's not just about comparing outputs but your AI workflow might
[00:32:57] outputs but your AI workflow might change as part of it once we look at
[00:33:00] change as part of it once we look at tokens and how long things take when
[00:33:02] tokens and how long things take when compared against both of them. So we're
[00:33:04] compared against both of them. So we're going to go through a little experiment
[00:33:05] going to go through a little experiment as well. All right. Now, it's funny
[00:33:07] as well. All right. Now, it's funny because everything we're just about to
[00:33:09] because everything we're just about to go through, I actually am refilming it
[00:33:11] go through, I actually am refilming it after I filmed it because I realized
[00:33:13] after I filmed it because I realized that I ran Claude on extra high, but I
[00:33:16] that I ran Claude on extra high, but I ran codeex just on high. So, there might
[00:33:19] ran codeex just on high. So, there might have been a little bit of a discrepancy
[00:33:20] have been a little bit of a discrepancy there. Um, just because after a bunch of
[00:33:23] there. Um, just because after a bunch of research two days ago, extra high and
[00:33:25] research two days ago, extra high and extra high on Claude and Codeex, the
[00:33:27] extra high on Claude and Codeex, the exact same things, even though Claude
[00:33:28] exact same things, even though Claude has a max plant. All right, so anyways,
[00:33:31] has a max plant. All right, so anyways, this is besides the point. What we're
[00:33:32] this is besides the point. What we're going to do is we're going to reuse that
[00:33:34] going to do is we're going to reuse that same prompt that we've been running with
[00:33:35] same prompt that we've been running with and I want to build a financial
[00:33:36] and I want to build a financial management app because I want to
[00:33:38] management app because I want to showcase sort of the outputs on codeex
[00:33:41] showcase sort of the outputs on codeex and also claude and to compare how long
[00:33:43] and also claude and to compare how long things take and also how many tokens are
[00:33:46] things take and also how many tokens are used. So let's go ahead and just run
[00:33:48] used. So let's go ahead and just run this. This is what we got back. Pretty
[00:33:50] this. This is what we got back. Pretty halfdeent looking uh dashboard all
[00:33:53] halfdeent looking uh dashboard all things considered. Um so what I want to
[00:33:56] things considered. Um so what I want to do now is I'm going to choose to push
[00:33:59] do now is I'm going to choose to push this to Figma. What I chose to do here
[00:34:01] this to Figma. What I chose to do here is I just chose to push it to Figma.
[00:34:03] is I just chose to push it to Figma. Now, one thing that's important to note
[00:34:04] Now, one thing that's important to note is you don't need to push everything to
[00:34:05] is you don't need to push everything to Figma in order to bring it into codeex.
[00:34:09] Figma in order to bring it into codeex. And especially for those more technical,
[00:34:10] And especially for those more technical, I know there's other ways to do things,
[00:34:12] I know there's other ways to do things, you know, get repo, bring it into
[00:34:14] you know, get repo, bring it into codeex. But this is the flow that
[00:34:17] codeex. But this is the flow that designers are generally most comfortable
[00:34:19] designers are generally most comfortable with. So that's what I'm going with for
[00:34:20] with. So that's what I'm going with for now because even with clawed code, we
[00:34:23] now because even with clawed code, we might want to push it to Figma, make
[00:34:25] might want to push it to Figma, make changes in Figma, and then bring it into
[00:34:27] changes in Figma, and then bring it into codeex. That's another big part of a
[00:34:29] codeex. That's another big part of a designer's workflow. It's not about
[00:34:30] designer's workflow. It's not about always going right from cloud code right
[00:34:33] always going right from cloud code right into codeex and then right back into
[00:34:35] into codeex and then right back into cloud code. We generally have that Figma
[00:34:37] cloud code. We generally have that Figma step in between, make tweaks, move
[00:34:39] step in between, make tweaks, move things around on our own, and then when
[00:34:40] things around on our own, and then when we're ready, we might pull it either
[00:34:42] we're ready, we might pull it either way. While that's being pushed to Figma
[00:34:44] way. While that's being pushed to Figma inside Cloud, I'm I'm going to hop into
[00:34:46] inside Cloud, I'm I'm going to hop into Codeex here and run the exact same
[00:34:47] Codeex here and run the exact same prompt we ran before. Uh, so let's go
[00:34:50] prompt we ran before. Uh, so let's go ahead and let's run this. This is what
[00:34:52] ahead and let's run this. This is what Codex came back with. I know what you're
[00:34:55] Codex came back with. I know what you're thinking. This is horrible. You're not
[00:34:56] thinking. This is horrible. You're not wrong. is not great is because for
[00:34:59] wrong. is not great is because for designs out of the box, codeex is not
[00:35:01] designs out of the box, codeex is not your best option. Claude is you could
[00:35:04] your best option. Claude is you could see the design that Claude produced
[00:35:05] see the design that Claude produced compared to this night and day
[00:35:07] compared to this night and day difference. But that doesn't mean that
[00:35:09] difference. But that doesn't mean that we shouldn't use codeex as part of our
[00:35:12] we shouldn't use codeex as part of our AI workflow. I'm going to elaborate on
[00:35:15] AI workflow. I'm going to elaborate on this, but what I want to do now is with
[00:35:17] this, but what I want to do now is with the design that Claude had pushed to
[00:35:20] the design that Claude had pushed to Figma,
[00:35:21] Figma, I'm going to bring that into codeex. And
[00:35:24] I'm going to bring that into codeex. And you're going to see why. And this is
[00:35:26] you're going to see why. And this is here is what Claude gave us inside
[00:35:27] here is what Claude gave us inside Figma. It looks pretty good. There's
[00:35:29] Figma. It looks pretty good. There's there were some small responsiveness
[00:35:30] there were some small responsiveness things that I noticed, but that's
[00:35:32] things that I noticed, but that's totally fine. What I'm going to do, I'm
[00:35:33] totally fine. What I'm going to do, I'm going to copy a link to this section.
[00:35:35] going to copy a link to this section. And then let's go and go back to codeex
[00:35:38] And then let's go and go back to codeex inside codeex. I'm going to paste a link
[00:35:39] inside codeex. I'm going to paste a link to that Figma file. Please build this uh
[00:35:44] to that Figma file. Please build this uh locally. And we're going to have codeex
[00:35:47] locally. And we're going to have codeex build what Claude had. So, let's run.
[00:35:49] build what Claude had. So, let's run. All right. This is what Codex came back
[00:35:51] All right. This is what Codex came back with. What I want to do now is I want to
[00:35:55] with. What I want to do now is I want to make the same changes on codeex and on
[00:35:59] make the same changes on codeex and on claude and compare how long those
[00:36:01] claude and compare how long those changes take and also how many tokens
[00:36:05] changes take and also how many tokens that those changes burn.
[00:36:07] that those changes burn. So what are some larger and smaller
[00:36:09] So what are some larger and smaller changes? One, maybe we can swap this
[00:36:12] changes? One, maybe we can swap this from dark mode to light mode. Maybe we
[00:36:14] from dark mode to light mode. Maybe we want to make top households have a
[00:36:18] want to make top households have a search bar and make it full width. So
[00:36:21] search bar and make it full width. So some larger changes that come into play.
[00:36:25] some larger changes that come into play. What we're going to do, we're going to,
[00:36:26] What we're going to do, we're going to, as I mentioned, we're go to codeex, run
[00:36:28] as I mentioned, we're go to codeex, run that, and then go to claude and run
[00:36:29] that, and then go to claude and run those same instructions inside codeex.
[00:36:31] those same instructions inside codeex. Then running this prompt, make the
[00:36:33] Then running this prompt, make the following changes. Change it to light
[00:36:35] following changes. Change it to light mode. Add a search bar to top
[00:36:37] mode. Add a search bar to top households. Make top households full
[00:36:38] households. Make top households full width and swap positions of AUM and
[00:36:40] width and swap positions of AUM and households. So I decided to just swap
[00:36:42] households. So I decided to just swap these two cards along the top. Another
[00:36:44] these two cards along the top. Another smaller change. So let's copy this and
[00:36:47] smaller change. So let's copy this and then let's So first off, let's run this.
[00:36:51] then let's So first off, let's run this. There we go. Now let's copy this and
[00:36:53] There we go. Now let's copy this and let's do the same in cla back inside
[00:36:55] let's do the same in cla back inside claude. Then let's run the exact same
[00:36:57] claude. Then let's run the exact same thing. So we can see that it did its job
[00:36:58] thing. So we can see that it did its job versus the old version versus the new
[00:37:00] versus the old version versus the new version where it swapped households and
[00:37:01] version where it swapped households and aum. Here this version we have this full
[00:37:04] aum. Here this version we have this full width we have the search bar whereas on
[00:37:06] width we have the search bar whereas on the older version we did not. And those
[00:37:07] the older version we did not. And those changes have been made in claude as
[00:37:09] changes have been made in claude as well. One thing I'd like to call out is
[00:37:11] well. One thing I'd like to call out is the styling is different on Claude than
[00:37:14] the styling is different on Claude than Codex, but it's not Codex's fault
[00:37:16] Codex, but it's not Codex's fault because when the design was pushed from
[00:37:17] because when the design was pushed from Claude into Figma, it lost some of the
[00:37:20] Claude into Figma, it lost some of the good text treatment. So that's why on
[00:37:23] good text treatment. So that's why on Codeex how it looks a little bit plain.
[00:37:25] Codeex how it looks a little bit plain. Codex actually did a really good job
[00:37:27] Codex actually did a really good job taking exactly what was in Figma and
[00:37:28] taking exactly what was in Figma and bringing it into Codex. But think like
[00:37:30] bringing it into Codex. But think like styling just got lost in the Figma
[00:37:32] styling just got lost in the Figma translation. One thing I'd like to call
[00:37:33] translation. One thing I'd like to call out is right after I started filming
[00:37:36] out is right after I started filming some of these things, I realized I
[00:37:37] some of these things, I realized I forgot to take screenshots or showcase
[00:37:39] forgot to take screenshots or showcase what the context window was before and
[00:37:41] what the context window was before and after. So instead of having to like get
[00:37:43] after. So instead of having to like get my recording started, which takes like a
[00:37:45] my recording started, which takes like a couple seconds, I just screenshot it
[00:37:47] couple seconds, I just screenshot it right away just so it was fully honest
[00:37:49] right away just so it was fully honest what those tokens were that they started
[00:37:50] what those tokens were that they started with. All right. Um, but the time it t
[00:37:53] with. All right. Um, but the time it t it took to make those changes in Claude
[00:37:56] it took to make those changes in Claude was 12 minutes with 38,000 tokens used.
[00:38:01] was 12 minutes with 38,000 tokens used. The time it took in Codeex to make those
[00:38:03] The time it took in Codeex to make those changes was four minutes with 17,000
[00:38:06] changes was four minutes with 17,000 tokens used.
[00:38:09] tokens used. There is a lesson in this. And one of
[00:38:11] There is a lesson in this. And one of the reasons why I'm not showing like how
[00:38:14] the reasons why I'm not showing like how we're talking about how many tokens it
[00:38:15] we're talking about how many tokens it took to do everything so far is because
[00:38:18] took to do everything so far is because Claude had to build a design from
[00:38:20] Claude had to build a design from scratch, but Codex just brought in a
[00:38:21] scratch, but Codex just brought in a design from Figma. So, it's a little bit
[00:38:23] design from Figma. So, it's a little bit of a different workflow. So, you can't
[00:38:24] of a different workflow. So, you can't compare it apples to apples in that
[00:38:26] compare it apples to apples in that sense. So this is the results in that
[00:38:30] sense. So this is the results in that little experiment that we ran through.
[00:38:31] little experiment that we ran through. Clearly codeex is a little bit more
[00:38:33] Clearly codeex is a little bit more efficient in terms of time and also for
[00:38:35] efficient in terms of time and also for tokens. And as I said there's a lesson
[00:38:38] tokens. And as I said there's a lesson in this whereas if you want to maintain
[00:38:40] in this whereas if you want to maintain the best use of your time in tokens it
[00:38:42] the best use of your time in tokens it might make sense to generate an initial
[00:38:45] might make sense to generate an initial design in claw whether it's claw design
[00:38:47] design in claw whether it's claw design clawed code whatever it is. Again if
[00:38:49] clawed code whatever it is. Again if you're doing it in claw design you need
[00:38:51] you're doing it in claw design you need to bring it into claw code um in order
[00:38:54] to bring it into claw code um in order to push it to Figma. Then from cloud
[00:38:56] to push it to Figma. Then from cloud code, push it to Figma. Once it's in
[00:38:59] code, push it to Figma. Once it's in Figma, pull it from Figma into codeex
[00:39:01] Figma, pull it from Figma into codeex where you can iterate in codeex. You can
[00:39:03] where you can iterate in codeex. You can add different things, move things
[00:39:04] add different things, move things around, make tweaks like add a badge
[00:39:07] around, make tweaks like add a badge here, remove this badge, move this down,
[00:39:09] here, remove this badge, move this down, swap these elements, and then when
[00:39:11] swap these elements, and then when you're happy with it, push it to Figma
[00:39:13] you're happy with it, push it to Figma and then bring it back into Claude when
[00:39:16] and then bring it back into Claude when you're ready to make your developers
[00:39:18] you're ready to make your developers happy and to do any, you know, larger
[00:39:20] happy and to do any, you know, larger exploration. It doesn't make sense to
[00:39:25] exploration. It doesn't make sense to always make small changes in codecs.
[00:39:29] always make small changes in codecs. You're going to need to decide when it
[00:39:31] You're going to need to decide when it makes sense to do something in cloud
[00:39:32] makes sense to do something in cloud versus do something in codeex because it
[00:39:34] versus do something in codeex because it takes effort to either download the
[00:39:36] takes effort to either download the entire file, bring it into codeex or
[00:39:40] entire file, bring it into codeex or push to Figma and then bring it into
[00:39:42] push to Figma and then bring it into codeex from Figma.
[00:39:44] codeex from Figma. You know, it's it takes a lot of effort
[00:39:47] You know, it's it takes a lot of effort if especially if you're going down the
[00:39:48] if especially if you're going down the Figma route. it takes a lot of tokens.
[00:39:52] Figma route. it takes a lot of tokens. So if you need to just change one color
[00:39:54] So if you need to just change one color and move two things around, maybe it
[00:39:55] and move two things around, maybe it just does make sense to do it in cloud.
[00:39:57] just does make sense to do it in cloud. But if you have a full application in
[00:39:58] But if you have a full application in cloud code, then maybe it does make
[00:40:01] cloud code, then maybe it does make sense to bring things into codeex
[00:40:05] sense to bring things into codeex because you can make a lot of those
[00:40:07] because you can make a lot of those larger changes like if you have to spend
[00:40:08] larger changes like if you have to spend a couple days making a bunch of changes,
[00:40:10] a couple days making a bunch of changes, yeah, then sure it makes sense to do it
[00:40:12] yeah, then sure it makes sense to do it in codeex versus claude. But one or two
[00:40:14] in codeex versus claude. But one or two small things, you know, you don't always
[00:40:16] small things, you know, you don't always need to bring it into codeex. Right now,
[00:40:17] need to bring it into codeex. Right now, we're at a point where we understand
[00:40:19] we're at a point where we understand some of the key tools in the AI design
[00:40:21] some of the key tools in the AI design space right now. What that workflow
[00:40:23] space right now. What that workflow could look like depending where you are
[00:40:26] could look like depending where you are in your design journey, but I want to
[00:40:29] in your design journey, but I want to talk about design system consistency and
[00:40:31] talk about design system consistency and how our design systems work with these
[00:40:34] how our design systems work with these AI tools and how can we get a really
[00:40:36] AI tools and how can we get a really good result
[00:40:38] good result using AI when building designs, but
[00:40:41] using AI when building designs, but still keeping it on brand with our
[00:40:43] still keeping it on brand with our design system and the guidelines that
[00:40:45] design system and the guidelines that organization sets. I want to start off
[00:40:47] organization sets. I want to start off in claw design because this is getting
[00:40:50] in claw design because this is getting all the hype and I see a lot of
[00:40:51] all the hype and I see a lot of clickbait around this. So, I want to
[00:40:53] clickbait around this. So, I want to clear some things up. Now, one thing I'd
[00:40:56] clear some things up. Now, one thing I'd like to call out just as a fun fact,
[00:40:59] like to call out just as a fun fact, I've been filming everything you've seen
[00:41:01] I've been filming everything you've seen so far for seven hours because I do
[00:41:05] so far for seven hours because I do retakes all this stuff. It takes a long
[00:41:07] retakes all this stuff. It takes a long time in case you want a sneak preview
[00:41:09] time in case you want a sneak preview into my life behind the scenes. Um, so,
[00:41:12] into my life behind the scenes. Um, so, and also too, planning things. I'll like
[00:41:14] and also too, planning things. I'll like plan the video, then I realize I should
[00:41:15] plan the video, then I realize I should have done it a little bit differently or
[00:41:16] have done it a little bit differently or explain things differently. Takes a long
[00:41:19] explain things differently. Takes a long time. So, here just under design
[00:41:22] time. So, here just under design systems, we already have this design
[00:41:23] systems, we already have this design system in here, but I'm going to choose
[00:41:24] system in here, but I'm going to choose to create a new one. I'm going to create
[00:41:27] to create a new one. I'm going to create a design system. And what I did offline
[00:41:30] a design system. And what I did offline is I downloaded a copy of one of the
[00:41:34] is I downloaded a copy of one of the design systems that that's a part of our
[00:41:35] design systems that that's a part of our academy. Again, link in the in the
[00:41:37] academy. Again, link in the in the description. I'm just going to drag that
[00:41:39] description. I'm just going to drag that in. It's a real simple design system.
[00:41:41] in. It's a real simple design system. There's nothing crazy about it. There's
[00:41:44] There's nothing crazy about it. There's not these big insane UI layouts and
[00:41:47] not these big insane UI layouts and these super uber complex components with
[00:41:50] these super uber complex components with crazy blur. It's relatively simple
[00:41:52] crazy blur. It's relatively simple design system. So, I'm going to choose
[00:41:54] design system. So, I'm going to choose to attach all the pages and frames. And
[00:41:57] to attach all the pages and frames. And I can see that that's attached. Let's
[00:41:59] I can see that that's attached. Let's continue to the generation now. And
[00:42:02] continue to the generation now. And let's of course choose to generate.
[00:42:03] let's of course choose to generate. Forgot that was going to be a step. It
[00:42:05] Forgot that was going to be a step. It came back with
[00:42:07] came back with everything in the design system for us
[00:42:09] everything in the design system for us to preview. Say whether it looks good.
[00:42:11] to preview. Say whether it looks good. Say whether it needs work.
[00:42:13] Say whether it needs work. This just got in way better in the last
[00:42:16] This just got in way better in the last two and a half weeks or three weeks
[00:42:19] two and a half weeks or three weeks since whatever claw design was launched.
[00:42:22] since whatever claw design was launched. But there's still fundamental issues
[00:42:25] But there's still fundamental issues with it not capturing
[00:42:28] with it not capturing everything.
[00:42:30] everything. Let's go into buttons as an example.
[00:42:33] Let's go into buttons as an example. Now,
[00:42:34] Now, I know this design system really well
[00:42:37] I know this design system really well because I built it. We don't use ghost.
[00:42:41] because I built it. We don't use ghost. Oops. Sorry. We don't use ghost as
[00:42:45] Oops. Sorry. We don't use ghost as variant name. We don't use danger. We're
[00:42:49] variant name. We don't use danger. We're missing varants here.
[00:42:52] missing varants here. So, it appears as if there's still,
[00:42:54] So, it appears as if there's still, again, this is still better than what it
[00:42:56] again, this is still better than what it gave me like two weeks ago, but it
[00:42:58] gave me like two weeks ago, but it appears it's still missing things and
[00:43:01] appears it's still missing things and it's not capturing everything inside of
[00:43:03] it's not capturing everything inside of that design system.
[00:43:06] that design system. And another example here is we go up to
[00:43:08] And another example here is we go up to our type scale. I don't have a display
[00:43:12] our type scale. I don't have a display in this design system. I have a hero.
[00:43:14] in this design system. I have a hero. Where's our heading three? We just have
[00:43:17] Where's our heading three? We just have heading two, heading four. What about
[00:43:19] heading two, heading four. What about heading five, heading six? What about
[00:43:21] heading five, heading six? What about our body large, body, and body medium?
[00:43:26] our body large, body, and body medium? It's missing some of the most
[00:43:29] It's missing some of the most fundamental elements of the design
[00:43:31] fundamental elements of the design system because some of these I care less
[00:43:35] system because some of these I care less about and we're still missing a lot of
[00:43:36] about and we're still missing a lot of components like this is not the extent
[00:43:38] components like this is not the extent of all the components that are in that
[00:43:40] of all the components that are in that design system
[00:43:42] design system but I care less about avatars
[00:43:45] but I care less about avatars which also aren't correct. I care more
[00:43:49] which also aren't correct. I care more about form fields which look pretty
[00:43:51] about form fields which look pretty good. Sure, we're missing some variants
[00:43:53] good. Sure, we're missing some variants but that's fine. I care most about
[00:43:55] but that's fine. I care most about buttons. Again, these are look pretty
[00:43:57] buttons. Again, these are look pretty looking pretty good. Still missing some
[00:43:59] looking pretty good. Still missing some variance. And I care most about our
[00:44:01] variance. And I care most about our type. So, if I was cla or anthropic, I
[00:44:06] type. So, if I was cla or anthropic, I would focus most on the fundamentals and
[00:44:09] would focus most on the fundamentals and make sure that those are built properly.
[00:44:11] make sure that those are built properly. Now, one thing that's important to note
[00:44:13] Now, one thing that's important to note here is I can say this looks good or I
[00:44:15] here is I can say this looks good or I can say what needs work and I can
[00:44:16] can say what needs work and I can describe what I prefer, what they got
[00:44:18] describe what I prefer, what they got wrong,
[00:44:20] wrong, but I don't want to have to go through
[00:44:22] but I don't want to have to go through and do that for everything here. Doing
[00:44:26] and do that for everything here. Doing that alone, we already know how slow
[00:44:28] that alone, we already know how slow Claude is. Do we really have 12 hours to
[00:44:31] Claude is. Do we really have 12 hours to burn and all of our claw design credits
[00:44:34] burn and all of our claw design credits to go through and polish our design
[00:44:36] to go through and polish our design system? So just because you can import
[00:44:38] system? So just because you can import your design system
[00:44:40] your design system in theory you can but it doesn't mean
[00:44:44] in theory you can but it doesn't mean it's ready to produce these clawed
[00:44:46] it's ready to produce these clawed design highquality
[00:44:49] design highquality UIs with it's just not there yet. And
[00:44:52] UIs with it's just not there yet. And something you might get from your team
[00:44:54] something you might get from your team and it's something I know that some of
[00:44:55] and it's something I know that some of our community members have also got from
[00:44:57] our community members have also got from their team are is their bosses being
[00:45:00] their team are is their bosses being like just import the design system to
[00:45:02] like just import the design system to claude design. I don't know what the big
[00:45:03] claude design. I don't know what the big deal is. If you ever get that, just send
[00:45:07] deal is. If you ever get that, just send them this part of the video or recreate
[00:45:09] them this part of the video or recreate the demo in on its own where you upload
[00:45:11] the demo in on its own where you upload your design system and show them the
[00:45:13] your design system and show them the flaws. A lot of higher level design
[00:45:16] flaws. A lot of higher level design leadership execs, they're just going off
[00:45:18] leadership execs, they're just going off the headlines. They're going off the
[00:45:20] the headlines. They're going off the clickbait because they've never actually
[00:45:21] clickbait because they've never actually tried it themselves. So even though your
[00:45:24] tried it themselves. So even though your boss might be telling you just import
[00:45:26] boss might be telling you just import the design system to claw design, it's
[00:45:28] the design system to claw design, it's important for them to understand that it
[00:45:30] important for them to understand that it does not work as intended. All that's
[00:45:33] does not work as intended. All that's justice, not just to say that AI can't
[00:45:36] justice, not just to say that AI can't do anything relating to your design
[00:45:38] do anything relating to your design system entirely.
[00:45:40] system entirely. But what I'm going to show you, I want
[00:45:42] But what I'm going to show you, I want to talk about why it's not the best is I
[00:45:44] to talk about why it's not the best is I want to enter a link to just a empty
[00:45:46] want to enter a link to just a empty Figma file. It's just called variables
[00:45:47] Figma file. It's just called variables test. Please build me type styles. Uh,
[00:45:52] test. Please build me type styles. Uh, H1, H2,
[00:45:55] H1, H2, H2, H3, H4, H5, H6,
[00:46:00] H2, H3, H4, H5, H6, uh, P1, paragraph one, paragraph,
[00:46:04] uh, P1, paragraph one, paragraph, medium,
[00:46:06] medium, I spelled paragraph wrong, paragraph,
[00:46:08] I spelled paragraph wrong, paragraph, uh, medium,
[00:46:10] uh, medium, paragraph, uh, large, paragraph,
[00:46:14] paragraph, uh, large, paragraph, small
[00:46:16] small inside of this Figma file, inside of
[00:46:20] inside of this Figma file, inside of this Figma file.
[00:46:22] this Figma file. uh inside each style
[00:46:24] uh inside each style there should be uh variables. This file
[00:46:29] there should be uh variables. This file file above is empty. So you will need to
[00:46:36] file above is empty. So you will need to uh build the variables
[00:46:39] uh build the variables and styles
[00:46:41] and styles and styles.
[00:46:44] and styles. Let's run this. And this is the result.
[00:46:46] Let's run this. And this is the result. It looks pretty good. So we have all of
[00:46:48] It looks pretty good. So we have all of our variables inside of a typography
[00:46:50] our variables inside of a typography collection. I always like to call this
[00:46:51] collection. I always like to call this responsive actually. Uh again, some
[00:46:53] responsive actually. Uh again, some links in the description for responsive
[00:46:55] links in the description for responsive collection stuff. Um and we have all of
[00:46:57] collection stuff. Um and we have all of our styles on the right hand side here.
[00:46:59] our styles on the right hand side here. Looks like our variables are applied.
[00:47:01] Looks like our variables are applied. This is pretty good. Now let's do the
[00:47:04] This is pretty good. Now let's do the same but with our
[00:47:08] same but with our more color specific variables because I
[00:47:11] more color specific variables because I want to talk through some things here.
[00:47:13] want to talk through some things here. Back inside. Let's run another prompt.
[00:47:15] Back inside. Let's run another prompt. Now build me uh a complete uh Figma
[00:47:19] Now build me uh a complete uh Figma variable uh library.
[00:47:23] variable uh library. Spelled variable wrong. Come on. Oh, it
[00:47:25] Spelled variable wrong. Come on. Oh, it doesn't even come up. Uh variable
[00:47:27] doesn't even come up. Uh variable library. There we go. Uh let move my
[00:47:29] library. There we go. Uh let move my video bar out of the way. Perfect. I got
[00:47:31] video bar out of the way. Perfect. I got some space. Now build me a complete
[00:47:33] some space. Now build me a complete Figma variable library. We will use a
[00:47:36] Figma variable library. We will use a three uh tiered approach
[00:47:39] three uh tiered approach brand, alias, and matt collection.
[00:47:50] Brand has raw hex codes and groupings. Alias uh determines primary secondary
[00:47:56] Alias uh determines primary secondary etc. error
[00:47:58] etc. error etc.
[00:48:01] etc. Uh and mapped has all of the surface
[00:48:06] Uh and mapped has all of the surface text
[00:48:07] text uh icon and border variables
[00:48:12] uh icon and border variables inside uh alias collection.
[00:48:15] inside uh alias collection. There will be a second brand for a
[00:48:19] There will be a second brand for a second brand within there will be a
[00:48:22] second brand within there will be a second mode for a second brand
[00:48:26] second mode for a second brand inside of mapped is a second mode for
[00:48:31] inside of mapped is a second mode for dark mode. Uh uh so now build me a
[00:48:34] dark mode. Uh uh so now build me a complete fivariable library. We use a
[00:48:35] complete fivariable library. We use a three- tiered approach brand alias and
[00:48:37] three- tiered approach brand alias and map collection. Brand has raw hex codes
[00:48:39] map collection. Brand has raw hex codes and groupings. Alias determines primary
[00:48:40] and groupings. Alias determines primary secondary error and mapped has all the
[00:48:42] secondary error and mapped has all the surface text icon and border variables.
[00:48:44] surface text icon and border variables. Inside alias collection, there would be
[00:48:45] Inside alias collection, there would be a second mode for a second brand. Inside
[00:48:47] a second mode for a second brand. Inside of maps is a second mode for dark mode.
[00:48:49] of maps is a second mode for dark mode. If you don't know what any of these like
[00:48:50] If you don't know what any of these like collections are, again, link in the
[00:48:52] collections are, again, link in the description for some other videos that
[00:48:53] description for some other videos that goes through that in depth. Um, again, I
[00:48:56] goes through that in depth. Um, again, I kind of build design systems for a
[00:48:57] kind of build design systems for a living, so know a lot on that topic. But
[00:48:59] living, so know a lot on that topic. But let's run this. This is what it came
[00:49:01] let's run this. This is what it came back with. So, we have our mapped
[00:49:03] back with. So, we have our mapped collection, light and dark, our brand
[00:49:04] collection, light and dark, our brand collection with all those raw hex codes,
[00:49:05] collection with all those raw hex codes, our alias collection where we defined
[00:49:07] our alias collection where we defined primary and so on so forth. Brand A,
[00:49:09] primary and so on so forth. Brand A, brand B as modes. And again, our map
[00:49:11] brand B as modes. And again, our map collection as I said with light and
[00:49:12] collection as I said with light and dark. And this is where all the heart of
[00:49:13] dark. And this is where all the heart of the action is.
[00:49:16] the action is. This is one of those scenarios where
[00:49:19] This is one of those scenarios where just because AI can do it doesn't mean
[00:49:22] just because AI can do it doesn't mean you should use AI to do it. Because one
[00:49:27] you should use AI to do it. Because one thing I noticed here is there's no
[00:49:28] thing I noticed here is there's no disabled but no disabled variable for
[00:49:31] disabled but no disabled variable for disabled buttons. There's no disabled
[00:49:33] disabled buttons. There's no disabled border. We're missing some pretty key
[00:49:35] border. We're missing some pretty key variables as I scrolled through here.
[00:49:39] variables as I scrolled through here. also two at the same time. So AI gave
[00:49:42] also two at the same time. So AI gave you this variable library. Now what do
[00:49:45] you this variable library. Now what do you have to do? You have to start
[00:49:46] you have to do? You have to start building some components.
[00:49:49] building some components. How do you know which variables
[00:49:51] How do you know which variables to apply to which components? Which
[00:49:54] to apply to which components? Which variables you might be missing?
[00:49:57] variables you might be missing? This is one of those things where you're
[00:49:59] This is one of those things where you're going to spend more time trying to
[00:50:01] going to spend more time trying to figure out what it is that AI gave you
[00:50:04] figure out what it is that AI gave you than it would be to learn how to build
[00:50:06] than it would be to learn how to build your variable library properly. Spend
[00:50:09] your variable library properly. Spend three hours and we have complete free
[00:50:11] three hours and we have complete free videos on YouTube where we go through
[00:50:12] videos on YouTube where we go through that. Spend a couple hours, learn how to
[00:50:15] that. Spend a couple hours, learn how to build it so you can build it yourself
[00:50:18] build it so you can build it yourself and then scale up from there. This is
[00:50:21] and then scale up from there. This is one of those things where just because
[00:50:22] one of those things where just because AI can do it, you're going to spend more
[00:50:24] AI can do it, you're going to spend more time trying to figure out what it is AI
[00:50:25] time trying to figure out what it is AI gave you. If it's actually right, if it
[00:50:28] gave you. If it's actually right, if it has all the variables that you need, do
[00:50:30] has all the variables that you need, do all the variables that it gave you work
[00:50:31] all the variables that it gave you work with your brand? because maybe you don't
[00:50:33] with your brand? because maybe you don't need some of these and you won't know
[00:50:36] need some of these and you won't know until you start going through it. And I
[00:50:38] until you start going through it. And I see a lot of designers being like, "Oh,
[00:50:40] see a lot of designers being like, "Oh, I built my variable library with Claude
[00:50:41] I built my variable library with Claude code. Can you tell me what it is that
[00:50:43] code. Can you tell me what it is that I'm missing?" I have no idea. I'm not
[00:50:45] I'm missing?" I have no idea. I'm not your brand. Claude is not your brand.
[00:50:47] your brand. Claude is not your brand. They have no idea your brand guidelines,
[00:50:49] They have no idea your brand guidelines, your brand rules, what components you
[00:50:51] your brand rules, what components you need in your design system. Some brands
[00:50:53] need in your design system. Some brands will have way more complex components
[00:50:54] will have way more complex components that require a completely different
[00:50:57] that require a completely different range of variables.
[00:50:59] range of variables. It's difficult for AI to get this right.
[00:51:01] It's difficult for AI to get this right. back inside Claude. Let's have it build
[00:51:03] back inside Claude. Let's have it build us a component. Now, I had started
[00:51:05] us a component. Now, I had started filming, then I realized I actually
[00:51:07] filming, then I realized I actually said, "Build me a complete button
[00:51:08] said, "Build me a complete button variable set instead of component
[00:51:13] variable set instead of component and variant set uh based on the styles
[00:51:16] and variant set uh based on the styles and variables we created." Now, please
[00:51:18] and variables we created." Now, please build me a complete button component and
[00:51:19] build me a complete button component and variant set based on the styles and
[00:51:21] variant set based on the styles and variables we created. Sorry, but here
[00:51:22] variables we created. Sorry, but here get authenticity. I make mistakes as I
[00:51:23] get authenticity. I make mistakes as I go because I still want to have it
[00:51:26] go because I still want to have it generate us some components. I still
[00:51:27] generate us some components. I still want to talk through some things
[00:51:29] want to talk through some things associated with this. So let's go ahead
[00:51:31] associated with this. So let's go ahead and run this for this part of the lesson
[00:51:33] and run this for this part of the lesson is I'm using claw because it's more
[00:51:34] is I'm using claw because it's more accurate when working with Figma. I
[00:51:36] accurate when working with Figma. I didn't want to use codeex even though it
[00:51:38] didn't want to use codeex even though it takes less time, takes less tokens
[00:51:40] takes less time, takes less tokens because I know that codeex is going to
[00:51:41] because I know that codeex is going to give us like some iffy results and I
[00:51:43] give us like some iffy results and I also don't want to use sort of the less
[00:51:45] also don't want to use sort of the less superior tool to try to prove a point. I
[00:51:48] superior tool to try to prove a point. I want to go use Claude because I know
[00:51:49] want to go use Claude because I know it's going to give us a really great
[00:51:50] it's going to give us a really great result to show you what it can do but
[00:51:52] result to show you what it can do but still talk about why it's probably not
[00:51:54] still talk about why it's probably not the best approach to things. So just
[00:51:56] the best approach to things. So just wanted to clarify that why I'm using
[00:51:58] wanted to clarify that why I'm using Claude for this and not Codeex. Buttons
[00:52:00] Claude for this and not Codeex. Buttons are the easiest component of a design
[00:52:02] are the easiest component of a design system to build. Full stop. They're the
[00:52:03] system to build. Full stop. They're the easiest. And right now, we're at almost
[00:52:06] easiest. And right now, we're at almost at 6 minutes and 5,000 tokens to
[00:52:10] at 6 minutes and 5,000 tokens to generate our button component.
[00:52:13] generate our button component. This isn't even including any changes
[00:52:14] This isn't even including any changes that we're going to have to make. And
[00:52:15] that we're going to have to make. And look, it looks like it just came back
[00:52:17] look, it looks like it just came back and finished. So, 6 minutes and 5.4,000
[00:52:21] and finished. So, 6 minutes and 5.4,000 tokens. It's a lot of time. It's a lot
[00:52:23] tokens. It's a lot of time. It's a lot of tokens for the easiest component.
[00:52:25] of tokens for the easiest component. Let's jump into Figma now. This is what
[00:52:27] Let's jump into Figma now. This is what it came back with. So, we have our um
[00:52:29] it came back with. So, we have our um sort of gave us like this little demo
[00:52:31] sort of gave us like this little demo here, which is actually kind of nice. I
[00:52:32] here, which is actually kind of nice. I do like the thought here where what
[00:52:36] do like the thought here where what those buttons look like on light mode,
[00:52:37] those buttons look like on light mode, what they look like on dark mode. I
[00:52:39] what they look like on dark mode. I don't know for sure, but I don't know if
[00:52:40] don't know for sure, but I don't know if some of these would pass our color
[00:52:41] some of these would pass our color contrast ratios right away. This one
[00:52:44] contrast ratios right away. This one absolutely wouldn't. So, that would be
[00:52:46] absolutely wouldn't. So, that would be something that we would need to fix.
[00:52:48] something that we would need to fix. Now, another thing right off the bat is
[00:52:52] Now, another thing right off the bat is in the button component.
[00:52:54] in the button component. So we have like our primary, secondary,
[00:52:56] So we have like our primary, secondary, tertiary, and then our then our error.
[00:52:58] tertiary, and then our then our error. It went ahead and built us went ahead
[00:53:00] It went ahead and built us went ahead and built us a disabled button, which
[00:53:02] and built us a disabled button, which they also gave us some disabled
[00:53:04] they also gave us some disabled variables, which it missed originally,
[00:53:06] variables, which it missed originally, which is good.
[00:53:08] which is good. But
[00:53:10] But in our variables here, let's go in. We
[00:53:13] in our variables here, let's go in. We have a secondary variable set. We have a
[00:53:16] have a secondary variable set. We have a success warning variable set. It gave us
[00:53:20] success warning variable set. It gave us those examples here. Why aren't they in
[00:53:23] those examples here. Why aren't they in the button component?
[00:53:26] the button component? So, we just spent six minutes and like
[00:53:28] So, we just spent six minutes and like what was it like five plus thousand
[00:53:30] what was it like five plus thousand tokens to build this. Now, we need some
[00:53:33] tokens to build this. Now, we need some changes.
[00:53:34] changes. And that's the hard part with building
[00:53:36] And that's the hard part with building components with AI. You go through
[00:53:39] components with AI. You go through tokens. You go through a ton of time to
[00:53:43] tokens. You go through a ton of time to not get the result that you're looking
[00:53:45] not get the result that you're looking for the first time. And then you either
[00:53:47] for the first time. And then you either have to spend time fixing it yourself,
[00:53:49] have to spend time fixing it yourself, trying to reverse engineer what it is
[00:53:51] trying to reverse engineer what it is that AI gave you, or burning through
[00:53:55] that AI gave you, or burning through more tokens and more time trying to
[00:53:57] more tokens and more time trying to dialogue with the AI.
[00:54:01] dialogue with the AI. And for such a simple component, button
[00:54:04] And for such a simple component, button component, there are a million into
[00:54:07] component, there are a million into design systems out there that have
[00:54:09] design systems out there that have button components that have link
[00:54:11] button components that have link components, button icon components,
[00:54:13] components, button icon components, fields, labels, inputs. Why do we need
[00:54:16] fields, labels, inputs. Why do we need AI to do this for us? It is my honest
[00:54:19] AI to do this for us? It is my honest opinion that AI should be used for more
[00:54:22] opinion that AI should be used for more complex layouts and modals and dialogues
[00:54:26] complex layouts and modals and dialogues and things like that that actually take
[00:54:29] and things like that that actually take a lot of time to build because there's
[00:54:31] a lot of time to build because there's so many free resources and paid
[00:54:34] so many free resources and paid resources for 100 bucks, 120 bucks, 50
[00:54:36] resources for 100 bucks, 120 bucks, 50 bucks that offer you all of the
[00:54:40] bucks that offer you all of the components that AI is going to spend
[00:54:42] components that AI is going to spend time burning to get your design system
[00:54:44] time burning to get your design system up and running. and they're going to
[00:54:45] up and running. and they're going to come complete with a complete variable
[00:54:47] come complete with a complete variable set that are actually applied to real
[00:54:50] set that are actually applied to real components. So you're not having to rely
[00:54:52] components. So you're not having to rely on AI to build the variable set, then
[00:54:54] on AI to build the variable set, then build the components, then realize that
[00:54:56] build the components, then realize that it might have forgot some variables and
[00:54:58] it might have forgot some variables and then it needs to add more variables. And
[00:55:00] then it needs to add more variables. And it's just it's too much back and forth
[00:55:02] it's just it's too much back and forth that needs to happen. You're going to
[00:55:04] that needs to happen. You're going to spend more time reverse engineering
[00:55:05] spend more time reverse engineering things than anything else. And I get
[00:55:07] things than anything else. And I get emails like this, like AI gave me these
[00:55:09] emails like this, like AI gave me these variables. What am I missing? This
[00:55:11] variables. What am I missing? This proves my point is that if AI just gives
[00:55:14] proves my point is that if AI just gives you an output, but you don't know the
[00:55:16] you an output, but you don't know the rationale behind the output, how can you
[00:55:19] rationale behind the output, how can you expect to succeed in your day-to-day
[00:55:21] expect to succeed in your day-to-day design role when you're applying
[00:55:22] design role when you're applying variables that you didn't build when
[00:55:24] variables that you didn't build when you're using components that you didn't
[00:55:26] you're using components that you didn't build? So, you don't know how those
[00:55:28] build? So, you don't know how those variables are structured. You don't know
[00:55:29] variables are structured. You don't know which variables belong where. You don't
[00:55:30] which variables belong where. You don't know how those components were built.
[00:55:32] know how those components were built. So, if you need to make a change to
[00:55:33] So, if you need to make a change to those components, then you might mess
[00:55:35] those components, then you might mess something up. Now, the example that I
[00:55:37] something up. Now, the example that I gave is just for buttons, but again,
[00:55:38] gave is just for buttons, but again, that's just an easy component. there
[00:55:40] that's just an easy component. there were no atom components that went inside
[00:55:42] were no atom components that went inside of that component. So you all you have
[00:55:44] of that component. So you all you have to do is just change the one uh
[00:55:47] to do is just change the one uh component set and the variance inside.
[00:55:50] component set and the variance inside. But once you start getting into more
[00:55:51] But once you start getting into more complex things like tables, really
[00:55:54] complex things like tables, really complex component graphs where there's
[00:55:57] complex component graphs where there's all these different components that are
[00:55:58] all these different components that are added together to build one larger
[00:56:00] added together to build one larger component. That's where you're going to
[00:56:02] component. That's where you're going to run into a lot of issues where AI is
[00:56:04] run into a lot of issues where AI is going to give you something but you
[00:56:06] going to give you something but you don't know how it's structured and why
[00:56:07] don't know how it's structured and why it's built the way that it was. And I
[00:56:09] it's built the way that it was. And I said this in my last video. I have free
[00:56:12] said this in my last video. I have free videos teaching you how to do a lot of
[00:56:14] videos teaching you how to do a lot of these things. To build a design system,
[00:56:16] these things. To build a design system, to set up a design system, uh to build a
[00:56:19] to set up a design system, uh to build a responsive collection. I have a
[00:56:21] responsive collection. I have a three-hour video, and again, links in
[00:56:23] three-hour video, and again, links in the description for these on how to
[00:56:25] the description for these on how to build a design system. Three hours. Took
[00:56:27] build a design system. Three hours. Took me like 15 hours, 12 hours to film. All
[00:56:29] me like 15 hours, 12 hours to film. All right? So, there are resources available
[00:56:32] right? So, there are resources available for you. So, you don't need to rely on
[00:56:34] for you. So, you don't need to rely on AI for all these things. It might take
[00:56:35] AI for all these things. It might take you two to three hours, four hours, 5
[00:56:37] you two to three hours, four hours, 5 hours. This might take you an entire
[00:56:39] hours. This might take you an entire working day, but it's going to set you
[00:56:41] working day, but it's going to set you up for success if you know how to do it.
[00:56:43] up for success if you know how to do it. You can build it yourself. You don't
[00:56:44] You can build it yourself. You don't need to rely on AI for some of these
[00:56:46] need to rely on AI for some of these things. So, what you should ideally do
[00:56:48] things. So, what you should ideally do is start with a design system already.
[00:56:52] is start with a design system already. As I said earlier, there's a million and
[00:56:53] As I said earlier, there's a million and two templates out there. Some are free,
[00:56:56] two templates out there. Some are free, a lot are paid, but it's like they're
[00:56:57] a lot are paid, but it's like they're like 50 bucks, 100 bucks, 150 bucks for
[00:57:01] like 50 bucks, 100 bucks, 150 bucks for some really crazy design systems.
[00:57:03] some really crazy design systems. They're worth the investment, especially
[00:57:05] They're worth the investment, especially if you don't already have a design
[00:57:07] if you don't already have a design system or you have a design system, but
[00:57:10] system or you have a design system, but you need component references to go off
[00:57:12] you need component references to go off of. It's easy to plug and play with
[00:57:15] of. It's easy to plug and play with these systems. You can copy components
[00:57:17] these systems. You can copy components from one to another, recreate them,
[00:57:19] from one to another, recreate them, reverse engineer them, do whatever you
[00:57:20] reverse engineer them, do whatever you need to do. Don't rely on AI to build
[00:57:24] need to do. Don't rely on AI to build your simple components. It's a waste of
[00:57:25] your simple components. It's a waste of tokens. It's a waste of time. Trust me.
[00:57:28] tokens. It's a waste of time. Trust me. Because what I have in this design
[00:57:30] Because what I have in this design system, just as an example, again, this
[00:57:31] system, just as an example, again, this is the one that's part of our academy, I
[00:57:33] is the one that's part of our academy, I really cover like the fundamental
[00:57:35] really cover like the fundamental components, things like your the your
[00:57:37] components, things like your the your field and inputs, you know, your drop
[00:57:39] field and inputs, you know, your drop down, your text area, your checkbox.
[00:57:42] down, your text area, your checkbox. It's just all the components that every
[00:57:44] It's just all the components that every design system needs that you need to get
[00:57:46] design system needs that you need to get started. And you can imagine like it
[00:57:48] started. And you can imagine like it took 5 minutes for to generate just the
[00:57:52] took 5 minutes for to generate just the button components, but it didn't
[00:57:53] button components, but it didn't generate all the variants that we
[00:57:55] generate all the variants that we actually want. It didn't go through some
[00:57:56] actually want. It didn't go through some of that accessibility testing. So you
[00:57:58] of that accessibility testing. So you can imagine with all the components that
[00:57:59] can imagine with all the components that you're seeing here on the lefth hand
[00:58:00] you're seeing here on the lefth hand side. Sorry, I know my face is probably
[00:58:02] side. Sorry, I know my face is probably covering some of them that how many
[00:58:05] covering some of them that how many tokens and how many how much time it
[00:58:08] tokens and how many how much time it would take to go through and refine for
[00:58:10] would take to go through and refine for you to not even know if they're built
[00:58:11] you to not even know if they're built 100% right. So my suggestion you if
[00:58:14] 100% right. So my suggestion you if you're looking to build a design system
[00:58:15] you're looking to build a design system from scratch with AI, use your tokens
[00:58:18] from scratch with AI, use your tokens and time to build the larger, more
[00:58:20] and time to build the larger, more complex widgets, modules, dialogues,
[00:58:24] complex widgets, modules, dialogues, more complex layouts. Don't spend the
[00:58:27] more complex layouts. Don't spend the time building the easy components. Start
[00:58:29] time building the easy components. Start with a template of some sort or build
[00:58:31] with a template of some sort or build them yourself based on the tutorials
[00:58:33] them yourself based on the tutorials that we have in the description. When it
[00:58:35] that we have in the description. When it comes to working with AI on our design
[00:58:37] comes to working with AI on our design system is we need to train AI. It's not
[00:58:40] system is we need to train AI. It's not as easy as just, hey, this is my design
[00:58:43] as easy as just, hey, this is my design system. Build me these widgets. We need
[00:58:44] system. Build me these widgets. We need to guarantee that AI knows when to use
[00:58:48] to guarantee that AI knows when to use the variables, the styles, and when to
[00:58:50] the variables, the styles, and when to use the components. So, it's a little
[00:58:52] use the components. So, it's a little bit of a prompting workflow that you
[00:58:53] bit of a prompting workflow that you need to go through. So you need to train
[00:58:55] need to go through. So you need to train AI on your variables and styles first.
[00:58:57] AI on your variables and styles first. Sorry, I just say variables and styles,
[00:58:59] Sorry, I just say variables and styles, whatever. And then you need to train it
[00:59:00] whatever. And then you need to train it on your components and any related
[00:59:02] on your components and any related documentation that you have. Only then
[00:59:07] documentation that you have. Only then can you start to use AI to build a lot
[00:59:10] can you start to use AI to build a lot of those larger modules, widgets,
[00:59:12] of those larger modules, widgets, dialogues, leveraging the components and
[00:59:15] dialogues, leveraging the components and variables above because it's going to
[00:59:17] variables above because it's going to increase the chance that these here are
[00:59:20] increase the chance that these here are built the very first time correctly. So
[00:59:23] built the very first time correctly. So we don't have to burn through a million
[00:59:25] we don't have to burn through a million ADU tokens in 15 minutes to build
[00:59:27] ADU tokens in 15 minutes to build something that wasn't using what was
[00:59:29] something that wasn't using what was already in our design system. So it's a
[00:59:32] already in our design system. So it's a little bit of a prompting workflow that
[00:59:33] little bit of a prompting workflow that we need to go through. But it's
[00:59:35] we need to go through. But it's important that everything here is
[00:59:38] important that everything here is reusable.
[00:59:40] reusable. So what AI teaches itself, it can call
[00:59:42] So what AI teaches itself, it can call on that knowledge a little bit later on
[00:59:45] on that knowledge a little bit later on because every time we build one of
[00:59:46] because every time we build one of these, we don't want to have to revert
[00:59:48] these, we don't want to have to revert back to step one. We train it on our
[00:59:50] back to step one. We train it on our variables and our components and
[00:59:51] variables and our components and documentation. This is where custom
[00:59:53] documentation. This is where custom skills come into play. I'm actually
[00:59:55] skills come into play. I'm actually going to have a larger video talking
[00:59:56] going to have a larger video talking about how to best train AI on our design
[00:59:59] about how to best train AI on our design systems. I don't know if I could have
[01:00:00] systems. I don't know if I could have released it on YouTube or the academy,
[01:00:02] released it on YouTube or the academy, but we're going to go into the more
[01:00:03] but we're going to go into the more technical specifics of things. But in
[01:00:06] technical specifics of things. But in the meantime, what I suggest you do is
[01:00:10] the meantime, what I suggest you do is again starting off with training AI on
[01:00:12] again starting off with training AI on our variables in our styles
[01:00:16] our variables in our styles have some sort of template like this for
[01:00:18] have some sort of template like this for your variables where you have the token
[01:00:22] your variables where you have the token name or the variable name, it's value on
[01:00:24] name or the variable name, it's value on light mode is value on dark mode and a
[01:00:26] light mode is value on dark mode and a description of when it's used. And this
[01:00:29] description of when it's used. And this is why it's so important to build your
[01:00:32] is why it's so important to build your variables yourself because then you can
[01:00:34] variables yourself because then you can know explicitly when those variables are
[01:00:36] know explicitly when those variables are used and when those variables are not
[01:00:38] used and when those variables are not used. If you ask AI to do it, AI is
[01:00:40] used. If you ask AI to do it, AI is going to hallucinate. It might give
[01:00:44] going to hallucinate. It might give wrong use cases to when to use specific
[01:00:46] wrong use cases to when to use specific variables. It's not going to be perfect.
[01:00:49] variables. It's not going to be perfect. So spend a couple hours, learn to build
[01:00:51] So spend a couple hours, learn to build your variables correctly. Don't rely on
[01:00:53] your variables correctly. Don't rely on AI to do it. Now templates like these,
[01:00:56] AI to do it. Now templates like these, they're everywhere. This again, this is
[01:00:58] they're everywhere. This again, this is the one that's part of our academy, but
[01:01:00] the one that's part of our academy, but like there's a million and two free
[01:01:01] like there's a million and two free templates like that available on Figma
[01:01:03] templates like that available on Figma community. Heck, it's super easy to
[01:01:05] community. Heck, it's super easy to create your own. So, I'm not saying like
[01:01:07] create your own. So, I'm not saying like you need to use this by any means. It's
[01:01:09] you need to use this by any means. It's there there's ones that are free that
[01:01:11] there there's ones that are free that are out there. But again, just a
[01:01:13] are out there. But again, just a description, what the variable is, the
[01:01:15] description, what the variable is, the name, this value, light mode, value on
[01:01:17] name, this value, light mode, value on dark mode, and when it should be used.
[01:01:20] dark mode, and when it should be used. Because what we want to do now is just
[01:01:21] Because what we want to do now is just copy a link to this frame.
[01:01:24] copy a link to this frame. And we're going to work with AI to build
[01:01:26] And we're going to work with AI to build a skill
[01:01:28] a skill around these variables and when they're
[01:01:31] around these variables and when they're used that AI can reference every time
[01:01:33] used that AI can reference every time it's building something new. All right,
[01:01:34] it's building something new. All right, we're inside Claw. You can do this in
[01:01:36] we're inside Claw. You can do this in codeex 2 because both have the ability
[01:01:39] codeex 2 because both have the ability to build skills if you describe the type
[01:01:41] to build skills if you describe the type of skill that you want. Anyways, um
[01:01:45] of skill that you want. Anyways, um let's enter in a link to that uh Figma
[01:01:48] let's enter in a link to that uh Figma frame that we copied. Please
[01:01:51] frame that we copied. Please study all of the Figma variables
[01:01:56] study all of the Figma variables uh inside of this table. Uh after coming
[01:02:01] uh inside of this table. Uh after coming to an elite understanding
[01:02:13] on the variables, their values, their um naming and when they
[01:02:17] naming and when they are used, build a Claude skill
[01:02:23] are used, build a Claude skill that will help train Claude on when to
[01:02:29] that will help train Claude on when to use different variables for future
[01:02:34] use different variables for future designs. Please study all the variables
[01:02:37] designs. Please study all the variables inside of this table after coming to an
[01:02:38] inside of this table after coming to an elite understanding of the variables
[01:02:39] elite understanding of the variables they're naming and when they are used.
[01:02:41] they're naming and when they are used. Build a claude skill that will help
[01:02:42] Build a claude skill that will help train claude and when to use different
[01:02:43] train claude and when to use different variables for future designs. Um do not
[01:02:48] variables for future designs. Um do not uh include any type styles or type
[01:02:53] uh include any type styles or type specific variables
[01:02:55] specific variables uh inside of this skill. We're gonna get
[01:02:58] uh inside of this skill. We're gonna get to that very shortly. Uh only focus on
[01:03:02] to that very shortly. Uh only focus on surface,
[01:03:03] surface, border,
[01:03:05] border, uh text, and icon
[01:03:09] uh text, and icon variables. So, just some guardrails to
[01:03:10] variables. So, just some guardrails to set because sometimes it does include
[01:03:12] set because sometimes it does include type styles in this, but it's best to
[01:03:13] type styles in this, but it's best to include sort of another, um skill for
[01:03:16] include sort of another, um skill for the type styles. Let's go ahead and run
[01:03:19] the type styles. Let's go ahead and run this. This is what it came back with. As
[01:03:20] this. This is what it came back with. As I said earlier, I have more content
[01:03:22] I said earlier, I have more content coming. I don't know if it's for YouTube
[01:03:23] coming. I don't know if it's for YouTube or for the academy on the most ideal way
[01:03:26] or for the academy on the most ideal way to structure this, but it's still a
[01:03:27] to structure this, but it's still a couple things that I'm sorting through.
[01:03:29] couple things that I'm sorting through. I'm teaching you the method for now and
[01:03:31] I'm teaching you the method for now and things like that. Now, this is ideal.
[01:03:34] things like that. Now, this is ideal. What it actually did here is it gave us
[01:03:35] What it actually did here is it gave us back these individual markdown files
[01:03:38] back these individual markdown files where it goes through
[01:03:40] where it goes through where it sort of grouped everything. So,
[01:03:41] where it sort of grouped everything. So, we have our border, our icon, our
[01:03:43] we have our border, our icon, our surface, our text, when to use them, and
[01:03:45] surface, our text, when to use them, and what variables are available. And one
[01:03:47] what variables are available. And one thing that's really nice to Oh, perfect.
[01:03:48] thing that's really nice to Oh, perfect. Has some common pairings here, which is
[01:03:50] Has some common pairings here, which is great. Um, and what text does not have?
[01:03:53] great. Um, and what text does not have? Okay, beautiful. So, you even went deep
[01:03:55] Okay, beautiful. So, you even went deep here. So, there's no focus states on
[01:03:56] here. So, there's no focus states on text because again, you won't need a
[01:03:57] text because again, you won't need a focus state on text. Um, and then it has
[01:04:00] focus state on text. Um, and then it has the skill.mmd where it's sort of like
[01:04:02] the skill.mmd where it's sort of like how to choose the four different axises
[01:04:04] how to choose the four different axises and everything else and giving it a lot
[01:04:06] and everything else and giving it a lot of good context and when to essentially
[01:04:08] of good context and when to essentially reference each of those markdown files.
[01:04:10] reference each of those markdown files. This is really good for a first draft.
[01:04:13] This is really good for a first draft. You know, I've shown it a couple ways in
[01:04:14] You know, I've shown it a couple ways in the past where it doesn't go this level
[01:04:16] the past where it doesn't go this level of depthness. Is is that a word or this
[01:04:21] of depthness. Is is that a word or this doesn't go this indepth? Anyways, okay.
[01:04:25] doesn't go this indepth? Anyways, okay. So, this is good. And again, I'm going
[01:04:27] So, this is good. And again, I'm going to have more content coming on the right
[01:04:28] to have more content coming on the right way to structure this. Um, what we're
[01:04:30] way to structure this. Um, what we're going to do now is we're going to save
[01:04:31] going to do now is we're going to save this skill and let's flip back to Figma.
[01:04:35] this skill and let's flip back to Figma. What we want to do now do the same for a
[01:04:37] What we want to do now do the same for a type scale. This one's a little bit
[01:04:38] type scale. This one's a little bit different because
[01:04:41] different because there's no what word am I looking for?
[01:04:45] there's no what word am I looking for? description on when it should be used
[01:04:47] description on when it should be used because depending on the design that
[01:04:50] because depending on the design that you're going for, you might use an H3,
[01:04:53] you're going for, you might use an H3, you might use an H2, you might use an
[01:04:55] you might use an H2, you might use an H1, you might use an H1 regular, H1
[01:04:57] H1, you might use an H1 regular, H1 medium, H1 semi-bold.
[01:05:00] medium, H1 semi-bold. So, it's not like your variables where
[01:05:04] So, it's not like your variables where you have explicit rationale as to which
[01:05:07] you have explicit rationale as to which should be used where. If you have that
[01:05:10] should be used where. If you have that and your brand is that like
[01:05:13] and your brand is that like rigid, great, awesome.
[01:05:17] rigid, great, awesome. But it's pretty rare for that to happen.
[01:05:19] But it's pretty rare for that to happen. But what even though we don't have the
[01:05:21] But what even though we don't have the descriptions is we still want
[01:05:24] descriptions is we still want the AI to know what text styles and
[01:05:29] the AI to know what text styles and variables and things are available to
[01:05:31] variables and things are available to it. Because one thing that happens is if
[01:05:35] it. Because one thing that happens is if you just ask it to generate initial
[01:05:37] you just ask it to generate initial design as a mockup, it might use a size
[01:05:40] design as a mockup, it might use a size that's like 15, like a font size for
[01:05:42] that's like 15, like a font size for paragraph that's 15. And then when you
[01:05:44] paragraph that's 15. And then when you want to apply your design system to it,
[01:05:46] want to apply your design system to it, it never applies the design system to
[01:05:49] it never applies the design system to that one style because it's looking for
[01:05:52] that one style because it's looking for something in the text styles that
[01:05:53] something in the text styles that matches. And when there's no match
[01:05:55] matches. And when there's no match there, it just defaults to not applying
[01:05:58] there, it just defaults to not applying anything. So, we're going to still train
[01:06:01] anything. So, we're going to still train it on these text styles and text
[01:06:03] it on these text styles and text variables to let it know what's
[01:06:05] variables to let it know what's available to it. And what I mean by text
[01:06:08] available to it. And what I mean by text variables, sorry, I wasn't I wasn't
[01:06:09] variables, sorry, I wasn't I wasn't clear when I said that. Is inside of
[01:06:12] clear when I said that. Is inside of these, notice how we have variables
[01:06:13] these, notice how we have variables applied here. So, that's sort of what
[01:06:16] applied here. So, that's sort of what I'm I'm getting at. Again, I copied a
[01:06:18] I'm I'm getting at. Again, I copied a link to that frame. So, paste it in. Uh,
[01:06:21] link to that frame. So, paste it in. Uh, please study all of the uh text styles
[01:06:26] please study all of the uh text styles uh available
[01:06:29] uh available inside our design system here. Uh please
[01:06:34] inside our design system here. Uh please take note of all the variables applied
[01:06:38] take note of all the variables applied to the styles and their values on uh
[01:06:44] to the styles and their values on uh desktop uh and mobile.
[01:06:47] desktop uh and mobile. After coming to
[01:06:50] After coming to a complete understanding,
[01:06:59] uh please build a Claude skill
[01:07:02] a Claude skill which will inform Claude on
[01:07:06] which will inform Claude on which styles are available
[01:07:09] which styles are available on which styles are available. Please
[01:07:12] on which styles are available. Please study all the text styles available
[01:07:13] study all the text styles available inside our design system. uh above
[01:07:17] inside our design system. uh above above please take note of all the
[01:07:18] above please take note of all the variables applied to the styles and
[01:07:20] variables applied to the styles and their values in desktop and mobile after
[01:07:22] their values in desktop and mobile after coming to a complete understanding
[01:07:23] coming to a complete understanding please build a cla skill which will
[01:07:24] please build a cla skill which will inform clot in which styles are
[01:07:25] inform clot in which styles are available when building uh new designs
[01:07:29] available when building uh new designs something like that we could probably
[01:07:30] something like that we could probably clean it up a bit but let's run all
[01:07:32] clean it up a bit but let's run all right here we go rock and roll look what
[01:07:34] right here we go rock and roll look what it came back with
[01:07:36] it came back with another skill around our textiles it
[01:07:40] another skill around our textiles it gave us some stuff here too um but looks
[01:07:43] gave us some stuff here too um but looks pretty good I'm not going to go through
[01:07:44] pretty good I'm not going to go through That's a whole lot for me to read. I'm
[01:07:46] That's a whole lot for me to read. I'm going to make the assumption I did it
[01:07:47] going to make the assumption I did it right. You should not do that though.
[01:07:48] right. You should not do that though. You should always read through it. So,
[01:07:49] You should always read through it. So, we're going to go ahead again. Save this
[01:07:51] we're going to go ahead again. Save this skill. Let's talk about components now.
[01:07:53] skill. Let's talk about components now. Uh, field description skill must be 1024
[01:07:55] Uh, field description skill must be 1024 characters. Let's tell Claude it messed
[01:07:57] characters. Let's tell Claude it messed up. Okay. So, I'm going to fix that. It
[01:07:59] up. Okay. So, I'm going to fix that. It just just needs to be shorter. And then
[01:08:01] just just needs to be shorter. And then we're going to flip back to Figma to
[01:08:03] we're going to flip back to Figma to talk about components. Oftent times what
[01:08:05] talk about components. Oftent times what I see is designer just being like,
[01:08:07] I see is designer just being like, "Study my components." What happens then
[01:08:09] "Study my components." What happens then is the AI doesn't know where to look. It
[01:08:12] is the AI doesn't know where to look. It tries to go in a methodical order, but
[01:08:14] tries to go in a methodical order, but then all of a sudden, if there's atom
[01:08:15] then all of a sudden, if there's atom components on a page, it's going to
[01:08:19] components on a page, it's going to analyze the first component, realize
[01:08:20] analyze the first component, realize there's atom components that it needs to
[01:08:22] there's atom components that it needs to study, it's going to jump to that atom
[01:08:23] study, it's going to jump to that atom component, but then it's going to forget
[01:08:24] component, but then it's going to forget to work its way back to the other
[01:08:25] to work its way back to the other component to then continue what it was
[01:08:28] component to then continue what it was doing before, and it's going to miss
[01:08:30] doing before, and it's going to miss components because we just sort of gave
[01:08:32] components because we just sort of gave it as broad instruction with nothing to
[01:08:34] it as broad instruction with nothing to help guide it. And our goal here is when
[01:08:36] help guide it. And our goal here is when we say study my components is that it
[01:08:39] we say study my components is that it moves through different component groups
[01:08:41] moves through different component groups in a very methodical order and builds
[01:08:45] in a very methodical order and builds skills specific to these component
[01:08:47] skills specific to these component groups. It's going to help keep the AI
[01:08:50] groups. It's going to help keep the AI more on track when it's reviewing the
[01:08:52] more on track when it's reviewing the components and what's available and lead
[01:08:54] components and what's available and lead to a better output when we're asking a
[01:08:56] to a better output when we're asking a AI to generate designs. I want you to
[01:08:58] AI to generate designs. I want you to look here on the left hand side is we
[01:09:00] look here on the left hand side is we have different groups
[01:09:02] have different groups for our components. So we don't have
[01:09:04] for our components. So we don't have everything in a one long list. And the
[01:09:06] everything in a one long list. And the way I've grouped things is I have my
[01:09:08] way I've grouped things is I have my form elements with in field, input,
[01:09:09] form elements with in field, input, dropdown, text area, checkbox, radio
[01:09:11] dropdown, text area, checkbox, radio button, so on so forth. We have our
[01:09:12] button, so on so forth. We have our navigation components. And then we have
[01:09:14] navigation components. And then we have our data display components, things like
[01:09:17] our data display components, things like tables, tags, avatars, badges.
[01:09:21] tables, tags, avatars, badges. This is a really easy way of grouping
[01:09:24] This is a really easy way of grouping your components. These are sort of like
[01:09:26] your components. These are sort of like three broad categories that are kind of
[01:09:28] three broad categories that are kind of all-encompassing.
[01:09:29] all-encompassing. You don't have to use these same
[01:09:31] You don't have to use these same categories, but it's a way to approach
[01:09:33] categories, but it's a way to approach it and working to train the AI on your
[01:09:36] it and working to train the AI on your design system because when we
[01:09:39] design system because when we tell AI what to look at, we're going to
[01:09:42] tell AI what to look at, we're going to be specifying the individual groupings,
[01:09:44] be specifying the individual groupings, the form elements, the navigation, the
[01:09:45] the form elements, the navigation, the data display. We're not going to be
[01:09:47] data display. We're not going to be referencing the entire like list of
[01:09:49] referencing the entire like list of components because it can do that on its
[01:09:51] components because it can do that on its own. But these groupings help keep the
[01:09:54] own. But these groupings help keep the AI a little bit more structured. Let's
[01:09:56] AI a little bit more structured. Let's flip back to Claude. I'm going to paste
[01:09:57] flip back to Claude. I'm going to paste in a link to that Figma file. I didn't
[01:09:58] in a link to that Figma file. I didn't copy specific frame just that Figma
[01:10:00] copy specific frame just that Figma file. Please study the following
[01:10:04] file. Please study the following component
[01:10:06] component groupings.
[01:10:09] groupings. So form elements navigation
[01:10:12] So form elements navigation data display
[01:10:15] data display fixed that uh
[01:10:19] fixed that uh do not uh move to navigation elements
[01:10:24] do not uh move to navigation elements uh unless you have a mastery of form
[01:10:30] uh unless you have a mastery of form elements. The same with data display.
[01:10:36] elements. The same with data display. Only move once you have mastered the
[01:10:40] Only move once you have mastered the prior group.
[01:10:49] After coming to an elite understanding of all components,
[01:10:51] of all components, build a uh clawed skill
[01:10:55] build a uh clawed skill around which components
[01:10:58] around which components are available
[01:11:01] are available and
[01:11:02] and when to use them. Now, in the design
[01:11:05] when to use them. Now, in the design system, I don't have documentation. If
[01:11:07] system, I don't have documentation. If you did have documentation, this would
[01:11:09] you did have documentation, this would really come in handy, but we just don't
[01:11:11] really come in handy, but we just don't have it. So we're ask claw just to come
[01:11:13] have it. So we're ask claw just to come up around when these should be used. But
[01:11:15] up around when these should be used. But the components they're relatively
[01:11:16] the components they're relatively simple. So any AI should be able to
[01:11:19] simple. So any AI should be able to detect when to use a button versus a
[01:11:21] detect when to use a button versus a link. Um after coming to an elite
[01:11:22] link. Um after coming to an elite understanding of all components build a
[01:11:23] understanding of all components build a cloud skill around which components are
[01:11:25] cloud skill around which components are available and when to use them inside of
[01:11:27] available and when to use them inside of the skill. And what I want is I kind of
[01:11:29] the skill. And what I want is I kind of want to do it how we formatted the way
[01:11:31] want to do it how we formatted the way in which we did the variables where
[01:11:32] in which we did the variables where inside the skill there are different
[01:11:33] inside the skill there are different like files talking about the form
[01:11:35] like files talking about the form elements and navigation the data display
[01:11:36] elements and navigation the data display inside of the skill have different uh MD
[01:11:41] inside of the skill have different uh MD files talking about
[01:11:44] files talking about form elements navigation data display
[01:11:49] form elements navigation data display more in depth. It's just a way to
[01:11:51] more in depth. It's just a way to structure information. Again, I'm going
[01:11:53] structure information. Again, I'm going to have content coming out whether for
[01:11:54] to have content coming out whether for YouTube or the academy on how to better
[01:11:56] YouTube or the academy on how to better like do this and the right way to do
[01:11:58] like do this and the right way to do things going like a deeper level down,
[01:12:00] things going like a deeper level down, but I think this is good for now. So,
[01:12:01] but I think this is good for now. So, let's run. So, this is what it came back
[01:12:03] let's run. So, this is what it came back with took a little bit of time where we
[01:12:04] with took a little bit of time where we have, you know, our skill.md and then we
[01:12:06] have, you know, our skill.md and then we have uh individual MD files on
[01:12:09] have uh individual MD files on navigation form elements and data
[01:12:10] navigation form elements and data display talking about the variants that
[01:12:12] display talking about the variants that are there. One thing I actually should
[01:12:14] are there. One thing I actually should have done is in the original prompt I
[01:12:17] have done is in the original prompt I should have actually specified
[01:12:19] should have actually specified to also look at the properties the
[01:12:22] to also look at the properties the variance but as I took a quick look
[01:12:24] variance but as I took a quick look through it looked like it picked
[01:12:25] through it looked like it picked everything up but don't make that same
[01:12:27] everything up but don't make that same mistake just in case like Tom when like
[01:12:30] mistake just in case like Tom when like after coming to elite understanding of
[01:12:32] after coming to elite understanding of all components
[01:12:34] all components it should say components properties
[01:12:36] it should say components properties variants and anything else associated
[01:12:38] variants and anything else associated with it all right so something just to
[01:12:41] with it all right so something just to make sure that you don't make the same
[01:12:42] make sure that you don't make the same mistake take as because sometimes it can
[01:12:44] mistake take as because sometimes it can miss specific variants, but I think this
[01:12:46] miss specific variants, but I think this is good for the purpose of this
[01:12:47] is good for the purpose of this exercise. Let's go ahead. Let's save
[01:12:49] exercise. Let's go ahead. Let's save this skill and keep moving forward
[01:12:51] this skill and keep moving forward inside of just a new cla code session.
[01:12:53] inside of just a new cla code session. Then one thing I want to do is I just
[01:12:55] Then one thing I want to do is I just want to test out those skills, see how
[01:12:56] want to test out those skills, see how it looks. So, we haven't necessarily
[01:12:58] it looks. So, we haven't necessarily we're not building it out in Figma yet.
[01:13:00] we're not building it out in Figma yet. Let's just build a dashboard. Uh please
[01:13:03] Let's just build a dashboard. Uh please build actually let's go like simp sample
[01:13:05] build actually let's go like simp sample pricing page. Dashboard's too complex.
[01:13:07] pricing page. Dashboard's too complex. Um, please build me a simple uh pricing
[01:13:11] Um, please build me a simple uh pricing uh widget. Please reference
[01:13:14] uh widget. Please reference uh the components component and variable
[01:13:19] uh the components component and variable uh component type component type and
[01:13:23] uh component type component type and variables
[01:13:24] variables uh skills whatever something like that.
[01:13:27] uh skills whatever something like that. Let's just run it. It appears that the
[01:13:28] Let's just run it. It appears that the skills are being applied. The colors
[01:13:30] skills are being applied. The colors look right like the style looks right.
[01:13:31] look right like the style looks right. Sure it looks a little bit basic but
[01:13:33] Sure it looks a little bit basic but that's totally fine. We're going to
[01:13:35] that's totally fine. We're going to solve that a little bit later on.
[01:13:36] solve that a little bit later on. Something I want to touch upon with
[01:13:38] Something I want to touch upon with codeex now, especially if we're
[01:13:40] codeex now, especially if we're designing in clawed code or designing in
[01:13:42] designing in clawed code or designing in codeex, what we're about to chat about
[01:13:44] codeex, what we're about to chat about sort of applies to the other platform
[01:13:46] sort of applies to the other platform that we might not be using consistently.
[01:13:48] that we might not be using consistently. Based on what we looked at earlier is it
[01:13:51] Based on what we looked at earlier is it might make the most sense from like a
[01:13:53] might make the most sense from like a token perspective, from a efficiency
[01:13:55] token perspective, from a efficiency perspective to build something to start
[01:13:57] perspective to build something to start in Claude
[01:13:59] in Claude nice initial first draft and then move
[01:14:02] nice initial first draft and then move it to codeex for some of the refinement
[01:14:04] it to codeex for some of the refinement again a little bit more efficient on the
[01:14:05] again a little bit more efficient on the time and the tokens and then when ready
[01:14:07] time and the tokens and then when ready push it to Figma and then bring it back
[01:14:09] push it to Figma and then bring it back into claude.
[01:14:11] into claude. But we have our skills inside of Claude
[01:14:14] But we have our skills inside of Claude but not codeex.
[01:14:16] but not codeex. So there's no ability to keep the
[01:14:17] So there's no ability to keep the changes in sync with our design system
[01:14:19] changes in sync with our design system once we move everything into codeex and
[01:14:23] once we move everything into codeex and that's what I want to fix next. What I
[01:14:25] that's what I want to fix next. What I did is I just ran this prompt. I need
[01:14:27] did is I just ran this prompt. I need the design system components variables
[01:14:28] the design system components variables and text style skills and separate zip
[01:14:30] and text style skills and separate zip folders complete with all subfolders
[01:14:31] folders complete with all subfolders associated with this those skills and it
[01:14:34] associated with this those skills and it just packaged those up for me. So I can
[01:14:36] just packaged those up for me. So I can go ahead and download those and let's go
[01:14:37] go ahead and download those and let's go into codeex. What I did is just inside
[01:14:40] into codeex. What I did is just inside the codeex chat I provided those zip
[01:14:41] the codeex chat I provided those zip files and had it just create the skills
[01:14:44] files and had it just create the skills for me. So now there's a match between
[01:14:46] for me. So now there's a match between the skills that are in codeex and the
[01:14:48] the skills that are in codeex and the skills that are in in claude. So if we
[01:14:50] skills that are in in claude. So if we need to make changes in claude, it'll
[01:14:51] need to make changes in claude, it'll follow our design system. And if we need
[01:14:53] follow our design system. And if we need to make those changes in codeex, then
[01:14:55] to make those changes in codeex, then it'll also follow our design system.
[01:14:57] it'll also follow our design system. It's important to note though that
[01:14:59] It's important to note though that whenever you're modifying something, I
[01:15:02] whenever you're modifying something, I always still like to provide the design
[01:15:04] always still like to provide the design system file in case it does need to
[01:15:06] system file in case it does need to reference it. Just something to call
[01:15:08] reference it. Just something to call out. We'll look at that a little bit
[01:15:09] out. We'll look at that a little bit later. Now, when it comes to generating
[01:15:12] later. Now, when it comes to generating designs, AI always works better from
[01:15:14] designs, AI always works better from visuals.
[01:15:16] visuals. And I want you to pretend as if you're
[01:15:17] And I want you to pretend as if you're building a kitchen. Like you tell, you
[01:15:20] building a kitchen. Like you tell, you know, whoever's building your kitchen, I
[01:15:21] know, whoever's building your kitchen, I want my kitchen to be to be dark. They
[01:15:23] want my kitchen to be to be dark. They might give you this with this sort of
[01:15:26] might give you this with this sort of look with this type of cabinet.
[01:15:30] look with this type of cabinet. But you're like, no, I imagined this.
[01:15:33] But you're like, no, I imagined this. So, take like the same example and apply
[01:15:36] So, take like the same example and apply it to your AI.
[01:15:39] it to your AI. Unless the people building your kitchen
[01:15:40] Unless the people building your kitchen had a specific visual, there would be no
[01:15:42] had a specific visual, there would be no way for them to realize that this is
[01:15:44] way for them to realize that this is what you wanted in your kitchen. They
[01:15:46] what you wanted in your kitchen. They were just taking an accurate guess. And
[01:15:48] were just taking an accurate guess. And too often we assume that AI is going to
[01:15:50] too often we assume that AI is going to make the right call and we end up
[01:15:53] make the right call and we end up burning through tokens and we take way
[01:15:55] burning through tokens and we take way longer to get the design that we're
[01:15:58] longer to get the design that we're looking for. And the best way to tell AI
[01:16:00] looking for. And the best way to tell AI what we're looking for is to give it
[01:16:01] what we're looking for is to give it those specific examples. And the best
[01:16:03] those specific examples. And the best place to find those specific examples is
[01:16:05] place to find those specific examples is Mobin. And again, you can take 20% off
[01:16:07] Mobin. And again, you can take 20% off the annual mob and plan with the link
[01:16:08] the annual mob and plan with the link that's in the description. It's hands
[01:16:10] that's in the description. It's hands down one of my favorite tools in the
[01:16:11] down one of my favorite tools in the design space right now where essentially
[01:16:13] design space right now where essentially what it is just a massive repository of
[01:16:15] what it is just a massive repository of just about every screen app flow that's
[01:16:19] just about every screen app flow that's out there. So if I'm looking at Wise as
[01:16:21] out there. So if I'm looking at Wise as an example, like the money transfer app,
[01:16:22] an example, like the money transfer app, I can come in here see all of their
[01:16:24] I can come in here see all of their specific screens. I can break down those
[01:16:26] specific screens. I can break down those screens into specific flows. I can see
[01:16:28] screens into specific flows. I can see here on the lefth hand side where I'm
[01:16:30] here on the lefth hand side where I'm able to come in here and get a lot of
[01:16:31] able to come in here and get a lot of good inspiration and see how others are
[01:16:33] good inspiration and see how others are doing it. where, you know, this card
[01:16:35] doing it. where, you know, this card example, you know, maybe I like this
[01:16:37] example, you know, maybe I like this one. I can save this. I can copy it.
[01:16:39] one. I can save this. I can copy it. They have a Figma plugin where I can
[01:16:40] They have a Figma plugin where I can just take these screenshots and upload
[01:16:42] just take these screenshots and upload them right to Figma. It saves me a ton
[01:16:45] them right to Figma. It saves me a ton of time when it comes to designing. And
[01:16:47] of time when it comes to designing. And especially now with AI, what it allows
[01:16:49] especially now with AI, what it allows me to do is take screenshots of examples
[01:16:51] me to do is take screenshots of examples that I like. So AI has something to go
[01:16:54] that I like. So AI has something to go off of. So I found this example uh that
[01:16:57] off of. So I found this example uh that I like from this tool, Tonal. So what
[01:16:59] I like from this tool, Tonal. So what I'm going to do, I'm just going to come
[01:17:00] I'm going to do, I'm just going to come in here. I'm going to take a screenshot.
[01:17:01] in here. I'm going to take a screenshot. Again, you can download it, too, but I
[01:17:02] Again, you can download it, too, but I just always take screenshots. And then I
[01:17:04] just always take screenshots. And then I can see more like this down below. Um,
[01:17:07] can see more like this down below. Um, which is great because you you always
[01:17:08] which is great because you you always want to feed AI multiple different
[01:17:10] want to feed AI multiple different examples. Um, so you can come in here
[01:17:12] examples. Um, so you can come in here and take different screenshots of ones
[01:17:14] and take different screenshots of ones that are similar uh at least. But maybe
[01:17:17] that are similar uh at least. But maybe I want to use this as inspiration to
[01:17:19] I want to use this as inspiration to recreate one of these designs for me
[01:17:22] recreate one of these designs for me now. So, let's flip back to Claude. And
[01:17:24] now. So, let's flip back to Claude. And I believe I said it earlier, but with
[01:17:26] I believe I said it earlier, but with the link that's in the description, you
[01:17:27] the link that's in the description, you can take 20% off the annual mobin plan.
[01:17:29] can take 20% off the annual mobin plan. I've gotten to know the Mobin team
[01:17:31] I've gotten to know the Mobin team pretty well. Um, they're a great group.
[01:17:33] pretty well. Um, they're a great group. So definitely check out definitely
[01:17:34] So definitely check out definitely consider supporting them and checking
[01:17:36] consider supporting them and checking out Mobin if you haven't already. Um so
[01:17:38] out Mobin if you haven't already. Um so what I did here is I just dragged in a
[01:17:40] what I did here is I just dragged in a copy of the research that we got. This
[01:17:42] copy of the research that we got. This is going to help us inform claude code.
[01:17:45] is going to help us inform claude code. So using the screenshot uh attached or
[01:17:49] So using the screenshot uh attached or the uh reference example
[01:17:52] the uh reference example attached along with the uh variables
[01:17:59] attached along with the uh variables type styles
[01:18:06] and component skills skills. Please build
[01:18:10] skills. Please build please uh build a
[01:18:14] please uh build a page like this
[01:18:17] page like this uh using our uh design system. And one
[01:18:20] uh using our uh design system. And one thing I forgot to do that you should
[01:18:22] thing I forgot to do that you should always do is let me just uh go back to
[01:18:24] always do is let me just uh go back to Figma here. Sorry, I know you can't see
[01:18:26] Figma here. Sorry, I know you can't see it where I'm just going to copy a link
[01:18:29] it where I'm just going to copy a link to that Figma design system just in case
[01:18:32] to that Figma design system just in case it needs to reference it. It's always
[01:18:35] it needs to reference it. It's always best to do I always do it as best
[01:18:37] best to do I always do it as best practice. Here is our design uh system
[01:18:42] practice. Here is our design uh system uh uh file if you need it. But all info
[01:18:48] uh uh file if you need it. But all info uh should be encompassed
[01:18:53] uh should be encompassed inside uh the clawed skills inside the
[01:18:56] inside uh the clawed skills inside the claude skills. Again, it's just
[01:18:57] claude skills. Again, it's just something that I do even though we're
[01:18:58] something that I do even though we're not pushing to Figma yet. Um, it's just
[01:19:00] not pushing to Figma yet. Um, it's just something that I do. Do not push to
[01:19:03] something that I do. Do not push to Figma to Figma yet. Uh, simply just uh
[01:19:08] Figma to Figma yet. Uh, simply just uh generate it locally. So, what this is
[01:19:11] generate it locally. So, what this is doing is we're telling it what skills to
[01:19:14] doing is we're telling it what skills to reference so it doesn't miss any,
[01:19:15] reference so it doesn't miss any, especially if you have a lot of skills.
[01:19:17] especially if you have a lot of skills. Sometimes it might just skip over like a
[01:19:19] Sometimes it might just skip over like a textile skills. Again, we gave it the
[01:19:21] textile skills. Again, we gave it the design system file. It's just something
[01:19:23] design system file. It's just something I do. It's best practice. and then
[01:19:25] I do. It's best practice. and then telling it not to push to Figma yet
[01:19:27] telling it not to push to Figma yet because we want to see the design that
[01:19:29] because we want to see the design that it comes up with first and we can refine
[01:19:31] it comes up with first and we can refine it here or in codeex and bring it back
[01:19:33] it here or in codeex and bring it back here before we end up pushing to the
[01:19:36] here before we end up pushing to the actual Figma file. So let's go ahead
[01:19:37] actual Figma file. So let's go ahead let's run this. One thing I should have
[01:19:39] let's run this. One thing I should have also done is to specify that we don't
[01:19:41] also done is to specify that we don't want it a onetoone to what's here
[01:19:43] want it a onetoone to what's here because of course that's somebody else's
[01:19:44] because of course that's somebody else's design. Um and also too if you provide
[01:19:47] design. Um and also too if you provide more examples it's going to find some
[01:19:49] more examples it's going to find some synergies between those examples. Just
[01:19:51] synergies between those examples. Just something I want to call out. You
[01:19:52] something I want to call out. You shouldn't just provide mobin screenshots
[01:19:54] shouldn't just provide mobin screenshots and have Claude copy it one to one
[01:19:57] and have Claude copy it one to one because that's so that's someone else's
[01:19:58] because that's so that's someone else's design. So just wanted to call that out.
[01:20:00] design. So just wanted to call that out. While that's generating, we now have an
[01:20:02] While that's generating, we now have an example pretty similar to the one that
[01:20:04] example pretty similar to the one that we wanted. So you can see how just by
[01:20:07] we wanted. So you can see how just by giving AI an example or examples, we're
[01:20:11] giving AI an example or examples, we're able to get to our result first try
[01:20:12] able to get to our result first try because how difficult would the prompt
[01:20:15] because how difficult would the prompt be to sort of describe what we're
[01:20:17] be to sort of describe what we're looking for here? what we're looking for
[01:20:18] looking for here? what we're looking for here with all like like the the arc and
[01:20:21] here with all like like the the arc and this green you know dot here and
[01:20:23] this green you know dot here and everything sort of the layout it'd be
[01:20:26] everything sort of the layout it'd be one hefty prompt and a whole lot of back
[01:20:28] one hefty prompt and a whole lot of back and forth to try to get this but in that
[01:20:30] and forth to try to get this but in that one prompt by providing an example we
[01:20:32] one prompt by providing an example we were able to go from zero to 100
[01:20:34] were able to go from zero to 100 relatively quickly again ideally what we
[01:20:37] relatively quickly again ideally what we would want is for
[01:20:39] would want is for Claude to not produce something one to
[01:20:41] Claude to not produce something one to one but I'm just showing you the
[01:20:43] one but I'm just showing you the workflow here so always be sure to
[01:20:45] workflow here so always be sure to provide a couple examples so we can find
[01:20:47] provide a couple examples so we can find synergies
[01:20:48] synergies so that your design is more unique.
[01:20:50] so that your design is more unique. >> But there's an issue with this
[01:20:53] >> But there's an issue with this on our design system. We don't have
[01:20:56] on our design system. We don't have buttons that are this round.
[01:21:00] buttons that are this round. So something was clearly off there where
[01:21:03] So something was clearly off there where it didn't use like it said it used the
[01:21:06] it didn't use like it said it used the button, but it overrode
[01:21:08] button, but it overrode the corner radius variables on that
[01:21:11] the corner radius variables on that button to make it round.
[01:21:15] button to make it round. This is an example of what I mean where
[01:21:18] This is an example of what I mean where sometimes
[01:21:20] sometimes you're going to need to make tweaks in
[01:21:21] you're going to need to make tweaks in Claude. And if it's just this one tweak,
[01:21:25] Claude. And if it's just this one tweak, it doesn't make sense to push it into
[01:21:27] it doesn't make sense to push it into Figma, then bring it into codec codeex
[01:21:29] Figma, then bring it into codec codeex because that's a lot of tokens on its
[01:21:30] because that's a lot of tokens on its own. So instead of focusing on this
[01:21:34] own. So instead of focusing on this small item for now, what I might want to
[01:21:37] small item for now, what I might want to do if I was building out the rest of
[01:21:39] do if I was building out the rest of this onboarding, the rest of this
[01:21:40] this onboarding, the rest of this application, is get all the other pages
[01:21:43] application, is get all the other pages in there first. And then once I have all
[01:21:46] in there first. And then once I have all the other pages in there, I can take
[01:21:48] the other pages in there, I can take stock of what needs to change, how many
[01:21:51] stock of what needs to change, how many items need to change, what is wrong, and
[01:21:55] items need to change, what is wrong, and then once I have a list of everything
[01:21:56] then once I have a list of everything that needs to be changed, I can make the
[01:21:58] that needs to be changed, I can make the call as to whether it's worth the tokens
[01:22:01] call as to whether it's worth the tokens and worth the time to push everything
[01:22:03] and worth the time to push everything into Figma, then bring it into codeex to
[01:22:06] into Figma, then bring it into codeex to make those iterations,
[01:22:07] make those iterations, and then push it back to Figma, then
[01:22:09] and then push it back to Figma, then bring into clawed code when I'm ready.
[01:22:12] bring into clawed code when I'm ready. So just because some one item is off
[01:22:15] So just because some one item is off here does not mean that I need to go
[01:22:17] here does not mean that I need to go into codeex and make that change there
[01:22:19] into codeex and make that change there and then push it back for smaller
[01:22:21] and then push it back for smaller changes. It's still okay to work within
[01:22:24] changes. It's still okay to work within claude. But I think the best use of your
[01:22:27] claude. But I think the best use of your time here was to build is to build out
[01:22:29] time here was to build is to build out as much as you can in claude code. And
[01:22:31] as much as you can in claude code. And if there's a lot that needs to change,
[01:22:33] if there's a lot that needs to change, instead of smok focusing on those some
[01:22:34] instead of smok focusing on those some of those smaller items, you can do that
[01:22:37] of those smaller items, you can do that in codeex at a cheaper token cost and
[01:22:40] in codeex at a cheaper token cost and with less time and then bring back to
[01:22:41] with less time and then bring back to claude code when you're ready. I want to
[01:22:43] claude code when you're ready. I want to drop some knowledge on you here because
[01:22:46] drop some knowledge on you here because believe it or not, Mobin is actually not
[01:22:47] believe it or not, Mobin is actually not the only place to spin up inspiration
[01:22:51] the only place to spin up inspiration is here. We are just in chat GPT and I'm
[01:22:55] is here. We are just in chat GPT and I'm just going to configure this here. We're
[01:22:57] just going to configure this here. We're going to use thinking 5.5. We're not in
[01:22:59] going to use thinking 5.5. We're not in codeex. We're just in chat GPT.
[01:23:02] codeex. We're just in chat GPT. Something I want to let you know is what
[01:23:06] Something I want to let you know is what I'm going to do is I'm actually going to
[01:23:08] I'm going to do is I'm actually going to provide that screenshot here. Let me
[01:23:10] provide that screenshot here. Let me drag it in to chat GPT and say uh
[01:23:14] drag it in to chat GPT and say uh generate me some alternate options of a
[01:23:19] generate me some alternate options of a screen like uh this. Let's run. And look
[01:23:23] screen like uh this. Let's run. And look what we have now. we have a little bit
[01:23:25] what we have now. we have a little bit of a different option
[01:23:28] of a different option and a couple different options at that.
[01:23:31] and a couple different options at that. And this is what not a lot of people
[01:23:33] And this is what not a lot of people know is GPT 5.5
[01:23:37] know is GPT 5.5 is really good at generating
[01:23:41] is really good at generating designs based on even just a prompt or
[01:23:44] designs based on even just a prompt or other pieces of info that you give it.
[01:23:47] other pieces of info that you give it. It's really good and it's fast. It's
[01:23:50] It's really good and it's fast. It's included in your chat GBT plan. I've
[01:23:53] included in your chat GBT plan. I've gotten a lot of value from this so far.
[01:23:55] gotten a lot of value from this so far. We can change aspect ratios of things.
[01:23:57] We can change aspect ratios of things. We can um usually just there's usually a
[01:24:01] We can um usually just there's usually a Come on. What's going on here? There's
[01:24:02] Come on. What's going on here? There's us there's usually a little like um
[01:24:05] us there's usually a little like um pencil or highlight button where I can
[01:24:08] pencil or highlight button where I can highlight specific parts on an image
[01:24:09] highlight specific parts on an image that I want changed. Maybe it might just
[01:24:11] that I want changed. Maybe it might just be if I only have one generation, not
[01:24:13] be if I only have one generation, not multiple. But it's a wicked tool and
[01:24:16] multiple. But it's a wicked tool and something not a lot of designers know.
[01:24:18] something not a lot of designers know. So, what I'm going to do is I'm just
[01:24:19] So, what I'm going to do is I'm just going to download this image
[01:24:21] going to download this image specifically. Maybe we're going to have
[01:24:22] specifically. Maybe we're going to have Claude just adjust that initial design
[01:24:24] Claude just adjust that initial design so it's not like a onetoone match. Back
[01:24:26] so it's not like a onetoone match. Back inside Claude code, let's drag in that
[01:24:29] inside Claude code, let's drag in that uh item that we downloaded from chat
[01:24:31] uh item that we downloaded from chat GPT. Uh here is another example. I like
[01:24:36] GPT. Uh here is another example. I like can you tweak the design uh to match?
[01:24:40] can you tweak the design uh to match? And let's run. And look what it came
[01:24:41] And let's run. And look what it came back with now. A little bit of a
[01:24:42] back with now. A little bit of a different example. So it's not a
[01:24:43] different example. So it's not a onetoone match to what uh was there
[01:24:46] onetoone match to what uh was there before. Now some of the coloring is a
[01:24:48] before. Now some of the coloring is a little bit off. I could have been a
[01:24:49] little bit off. I could have been a little bit more explicit, but I think
[01:24:52] little bit more explicit, but I think that's fine. For the most part, it looks
[01:24:53] that's fine. For the most part, it looks as if it's following our design system
[01:24:55] as if it's following our design system relatively to a te and you know, I can't
[01:24:57] relatively to a te and you know, I can't really complain except with some of the
[01:24:59] really complain except with some of the smaller things as well. But again, now
[01:25:00] smaller things as well. But again, now that we have some of these colors and
[01:25:02] that we have some of these colors and this color isn't in our design system,
[01:25:03] this color isn't in our design system, something wrong with the button here.
[01:25:04] something wrong with the button here. Some of the radiuses are off as well.
[01:25:06] Some of the radiuses are off as well. Those are things that you know, if it's
[01:25:08] Those are things that you know, if it's worth it to bring it into codeex and
[01:25:09] worth it to bring it into codeex and make those changes, we can do that or we
[01:25:11] make those changes, we can do that or we can do it right here inside cloud code.
[01:25:13] can do it right here inside cloud code. What I want to do now, let me just copy
[01:25:15] What I want to do now, let me just copy a Figma link is uh sorry, here we go.
[01:25:17] a Figma link is uh sorry, here we go. back in Claude. Uh we're going to push
[01:25:19] back in Claude. Uh we're going to push this to Figma. Please push this to
[01:25:22] this to Figma. Please push this to Figma. Uh remember uh to follow uh the
[01:25:27] Figma. Uh remember uh to follow uh the design system
[01:25:29] design system and reference the text styles,
[01:25:34] and reference the text styles, variables, and component uh rules. So
[01:25:38] variables, and component uh rules. So let's run this. This is what's in Figma
[01:25:40] let's run this. This is what's in Figma now. Looks really good where we have,
[01:25:44] now. Looks really good where we have, you know, our different sections. It
[01:25:45] you know, our different sections. It looks like our styles and variables are
[01:25:46] looks like our styles and variables are all applied well, which is great. Um,
[01:25:49] all applied well, which is great. Um, you know, it didn't apply the drop
[01:25:51] you know, it didn't apply the drop shadow style, but it's just cuz I didn't
[01:25:52] shadow style, but it's just cuz I didn't include it in like one of those skills.
[01:25:54] include it in like one of those skills. Probably didn't know to look with look
[01:25:55] Probably didn't know to look with look for it. But you can see here that it's
[01:25:57] for it. But you can see here that it's using some of the raw the the instances
[01:26:02] using some of the raw the the instances of the components. Now, personally, I
[01:26:03] of the components. Now, personally, I would have loved for this to be a radio
[01:26:04] would have loved for this to be a radio button label and not just the radio
[01:26:06] button label and not just the radio button, but that's okay. Not the end of
[01:26:08] button, but that's okay. Not the end of the world. And looks like our items are
[01:26:10] the world. And looks like our items are applied here. Let's see what color it
[01:26:12] applied here. Let's see what color it used here. there. So, it used Okay, so
[01:26:13] used here. there. So, it used Okay, so surface warning default subtle is the
[01:26:15] surface warning default subtle is the variable it used for this kind of
[01:26:16] variable it used for this kind of treatment. I think it looks pretty good,
[01:26:18] treatment. I think it looks pretty good, you know, and it fixed like the rounded
[01:26:20] you know, and it fixed like the rounded corner that I was that I was showing in
[01:26:23] corner that I was that I was showing in um Claw Code and it like adjusted it to
[01:26:26] um Claw Code and it like adjusted it to the actual button with the actual corner
[01:26:28] the actual button with the actual corner radius instead of it being round. It's
[01:26:30] radius instead of it being round. It's just a little bit curved. So, I think it
[01:26:31] just a little bit curved. So, I think it looks pretty good for the most part. One
[01:26:33] looks pretty good for the most part. One thing that's important to note here is
[01:26:34] thing that's important to note here is before if you were to bring this into
[01:26:35] before if you were to bring this into codeex, say you have a bunch of app
[01:26:37] codeex, say you have a bunch of app screens here, you need to make a bunch
[01:26:38] screens here, you need to make a bunch of changes and you don't want to do it
[01:26:40] of changes and you don't want to do it in Figma and you can bring it into
[01:26:42] in Figma and you can bring it into codeex then is one thing you want to do
[01:26:44] codeex then is one thing you want to do is make sure that a lot of the small
[01:26:47] is make sure that a lot of the small things are cleaned up like the styles
[01:26:48] things are cleaned up like the styles are applied, variables are applied
[01:26:51] are applied, variables are applied because you don't want to do that in
[01:26:52] because you don't want to do that in codeex. You can just do it right in
[01:26:53] codeex. You can just do it right in Figma itself and then bring it into
[01:26:55] Figma itself and then bring it into codeex or back to clawed code. So if you
[01:26:57] codeex or back to clawed code. So if you ever have to push a design to Figma, try
[01:26:59] ever have to push a design to Figma, try to polish it as much as you can in Figma
[01:27:01] to polish it as much as you can in Figma before you bring it back into Claude or
[01:27:03] before you bring it back into Claude or back into Codeex or, you know, back into
[01:27:05] back into Codeex or, you know, back into Claude as I mentioned. So just something
[01:27:07] Claude as I mentioned. So just something for you to be aware of. Hey, thanks for
[01:27:09] for you to be aware of. Hey, thanks for watching this one. Really appreciate you
[01:27:10] watching this one. Really appreciate you being here. Be sure to drop a subscribe.
[01:27:12] being here. Be sure to drop a subscribe. Um going to have a lot more content
[01:27:14] Um going to have a lot more content coming on working with Claude, codecs,
[01:27:17] coming on working with Claude, codecs, AI workflows, and a whole bunch of other
[01:27:19] AI workflows, and a whole bunch of other things as well. So be sure to, you know,
[01:27:21] things as well. So be sure to, you know, check the channel out, subscribe, check
[01:27:23] check the channel out, subscribe, check back in. Uh, also be sure to check out
[01:27:26] back in. Uh, also be sure to check out our website. A link for that is in the
[01:27:28] our website. A link for that is in the description where I'm sort of teaching
[01:27:29] description where I'm sort of teaching everyone about AI workflows, design,
[01:27:31] everyone about AI workflows, design, leadership, and a whole bunch of other
[01:27:33] leadership, and a whole bunch of other things as well. I'm always adding new
[01:27:34] things as well. I'm always adding new courses to this. Working on a user
[01:27:36] courses to this. Working on a user testing course right now as we speak. So
[01:27:37] testing course right now as we speak. So again, that link is just in the
[01:27:38] again, that link is just in the description. And yeah, thanks for being
[01:27:40] description. And yeah, thanks for being here. Be sure to share this video and
[01:27:41] here. Be sure to share this video and rock and roll. See you at the next one.

---
layout: post
title:  "1 Trick to hack mybatis SQl weakness"
date:   2016-04-27 18:30:46
categories: work
img_url: http://oa1f2pgjm.bkt.clouddn.com/blog/IMG_3207%20%282%29_%E5%89%AF%E6%9C%AC.jpg
shortcut: This article explains how to sort your SQL result by field...
---
Mybatis sucks. It does not work 100% with SQl, or MYSQL, or any standard SQL language you can imagine, but sometimes you have to code with it since the project you handed from your former co-worker is not owned by you.

So you can imagine how I felt when last week I found it surprisingly hard to do something that should be easily done by Hiberate.

What I tried to do was to sort some result by field from database.
For example, I have a table 'music', of which the arrtibutes are id, album_logo, artist_name, play_seconds, song_name, and file_url. I wanted to select certain ids from the table where the id is in a given set, AND the result should be sorted by id. Normally I could just type

	`SELECT id as music_id, album_logo,artist_name,play_seconds,song_name,file_url
		from music
		where id in (1,2,3,4...)
		order by field (id)`

And that's it. But it is not gonna happen when I used Mybatis. Why? Its code has different way to achieve that function, and the origin 'SQL' way is not the option.

Finally I figured how it worked. What you should do is:

	'<select id="yourDaoMethodName" resultType="HashMap">
		SELECT id as music_id, album_logo,artist_name,play_seconds,song_name,file_url
		from music
		where id in
		<foreach item="id" index="index" collection="musicIds" open="(" separator="," close=")">
			#{id}
		</foreach>
		and  `status` in (1,3)
		ORDER BY field(id,
		<foreach item="id" collection="musicIds" separator=",">
			#{id}
		</foreach>
		)
	</select>'//musicIds is the data structure input, which should be array or listArray.

Be careful about the comma, the brackets, the angle brackets，because it won't work if any of them is wrongly set!  

Mybatis claims itself to be flexible and free-style, but other than the fussy moves to install, it also takes a lot to figure its own way to do the SQl, which takes almost as long as the time you spend on origin SQL, and what you have got? Mechanical， hard-to-read-and-refactor code.

:(
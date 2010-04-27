!SLIDE
# Event Driven Programming

!SLIDE code

	@@@ javascript
	var result = db.query("select * from T");
	// Use result

!SLIDE bullets

* L1: 3 cycles
* L2: 14 cycles
* RAM: ~250 cycles
* Disk: ~41.000.000 cycles (~13.7ms read)
* Network: 240.000.000 cycles (~80ms latency)

Source: [What your computer does while you wait](http://duartes.org/gustavo/blog/post/what-your-computer-does-while-you-wait)

!SLIDE center full-page
![nginx apache reqs](nginx-apache-reqs-sec.png)

Source: http://blog.webfaction.com/a-little-holiday-present

!SLIDE center full-page
![nginx apache memory](nginx-apache-memory.png)

Source: http://blog.webfaction.com/a-little-holiday-present

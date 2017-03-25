# notes

----------------------------------------------------------------
Hadoop Definitive Guide Chapter 2 - Notes:
----------------------------------------------------------------

Parallelizing by yourself is difficult,

Parallelize manually
say you have 2 files of weather data and 2 machines
get maximum temperature for each year? + all years
File 1 has 10 GB
File 2 has 1 GB

How do you split it?
   Split it by 1 file per machine?
   		Bottlenecked by FIle1

   	What if we split it by chunks? 6GB for each machine?
   		-> How do we know that the maximum in each maximum corresponds to particular machine?

Parallelizing stuff manually is complicated.


Use Hadoop 
	-> Hadoop = Map + Reduce

	Both of the phase has the same datatype (key+value pair)
	Map (key,value) pair
	Reduce (key,value) pair









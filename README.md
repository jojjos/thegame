thegame
=======

temporary


=======
#
#
#  Fixa uppaal, semaforerna strulas
#  Båda kommer in samtidigt, usch hetrosex.
#
#




#
#
#      _._     _,-'""`-._
#     (,-.`._,'(       |\`-/|
#         `-.-' \ )-`( , o o)
#                `-   \`_`"'-
#
#
# Inlämningsuppgift >:3
# Grupp AwesomeCatz: 
# Kim Wahlman (a12kimwa@student.his.se) 
# Lukas Emanuelsson (a12lukem@student.his.se) 
# Johannes Sinander (b12johsi@student.his.se)
# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # 

resource UBprob()
const m : int := 5	  #number of men
const w : int := 5    #number of women	

sem Men_guard 	:= 0		# Semaphore for men
sem Women_guard := 0		# Semaphore for woman
sem mutex 		:= 1		# Semaphore for mutex
 
var man_nr   : int := 0
var woman_nr : int := 0
var dm 		 : int := 0
var dw		 : int := 0

writes("Hallo!\n");

procedure signal() 
	if(woman_nr = 0 and dm > 0) ->
		dm--
		V(Men_guard)
	[](man_nr = 0 and  dw > 0) ->
		dw--
		V(Women_guard)
	[](woman_nr > 0 or dm = 0) and (man_nr > 0 or dw = 0) ->
		V(mutex)
	fi
end signal

process Man(i := 1 to m)
	do true -> 
		P(mutex) 						#P(Men_guard)
		if woman_nr > 0 -> 				# if man_nr = 1 ->
			dm++
			V(mutex)
			P(Men_guard)
		fi
		man_nr++						#V(Men_guard)
		signal()
                      # enter bathroom
	                  # spend some time
			writes("\tMan:    ", man_nr, "\n")
			nap(int(random(500, 1000)))
			
                      # leave bathroom
                      # spend some time
		P(mutex)						#P(Men_guard)
		man_nr--;
			writes("\tMan:    ", man_nr, "\n")
		signal()
										#if man_nr = 0 ->
										#	V(Women_guard)
										#fi
										#V(Men_guard)
		nap(int(random(500, 1000)))
	od
end Man

process Woman(j := 1 to w)
	do true -> 
		P(mutex) 						#P(Men_guard)
		if man_nr > 0 -> 				# if man_nr = 1 ->
			dw++
			V(mutex)
			P(Women_guard)
		fi
		woman_nr++						#V(Men_guard)
		signal()
                      # enter bathroom
	                  # spend some time
			writes("\tWoMan:    ", woman_nr, "\n")
			nap(int(random(500, 1000)))
			
                      # leave bathroom
                      # spend some time
		P(mutex)						#P(Men_guard)
		woman_nr--;
			writes("\tWoMan:    ", woman_nr, "\n")
		signal()
										#if man_nr = 0 ->
										#	V(Women_guard)
										#fi
										#V(Men_guard)
		nap(int(random(500, 1000)))
	od
end Woman
end UBprob

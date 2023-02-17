data : training data
dataaligned : aligned training data

------------

Training 

------------
Steps 1 - 6 : dict and monophones0 were created using an external python script, the prl script provided had encoding issues. We used the bikdash arabic transliteration rules as described here: 

http://www.eiktub.com/guide.html

Main unresolved problem : Missing vowels. Arabic is written with an abjad i.e. ommiting some or all short vowels (written as diacritics) is valid. The vast majority of arabic texts only include diacritics when it is necessary to clear up ambiguity. Finding a fully diacritized arabic text would be the ideal, but since we weren't able to get our hands on a big enough corpus, maybe we could create it ourselves. Yes but unfortunately we have no access to the necessary equipment (an arabic keyboard). We tried some automatic diacritization software on the text but all were very inefficient and wrongly diacritized multiple letters per word, which ended up giving worse results (intutively, false information is more detrimental on the training process than omitted information)

step 7 : HLEd -l '*' -d dict -i phones0.mlf mkphones0.led  words.mlf

step 8 : Create proto file

step 9 : config1 file

step 10 : mkdir hmm0; HCompV -T 1 -C config1 -f 0.01 -m -S train.scp -M hmm0 proto

step 11 : 

#!/bin/sh
echo "" >./hmm0/hmmdefs #
head -n 3 ./hmm0/proto > ./hmm0/macros
cat ./hmm0/vFloors >> ./hmm0/macros
for w  in `cat ./monophones0`
do
 cat ./hmm0/proto | sed "s/proto/$w/g"|sed "1 d"|sed "1 d"|sed "1 d" >> ./hmm0/hmmdefs
done

step 12: first 3 training cycles
step 13 : silent model modification + 1 training cycle

step 14 : HLEd -l '*' -d dict -i phones1.mlf mkphones1.led  words.mlf

step 15 : 2 training cycles

step 16 : forced alignment script


------------

Testing 

------------

HResults -t -I test_words.mlf ./monophones1 recout.mlf

Results : 

SENT: %Correct=0.00 [H=0, S=20, N=20]
WORD: %Corr=49.76, Acc=22.27 [H=105, D=24, S=82, I=58, N=211]


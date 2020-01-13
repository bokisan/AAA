#音声とイラストと意味

from PIL import Image
import cv2
import sys
import pyocr
import pyocr.builders
import pyocr
from mutagen.mp3 import MP3 as mp3
import pygame
import time
from PIL import Image
import azure.cognitiveservices.speech as speechsdk
import re
import cv2
import random
import time
import msvcrt
import numpy as np
from operator import itemgetter
import tkinter as tk
from PIL import Image, ImageTk
from win32api import GetSystemMetrics

word_num = 70
score = [0] * (word_num+1)
word = {}
new_word = {}
new_score = {}
test_score = [0] * 10
#word[] = [単語0、音声1、イラスト2、テキスト画像3、意味4、認識単語5、説明6、カタカナ7]

#音声とイラストと意味 

word[0] = (["Eminence",r'practice_sound\\eminence.mp3', r'practice_image\\eminence.png', r'practice_text\\eminence_text.png', r'practice_imi\\eminence_imi.png',"Eminence.", r'practice_description\\eminence_des.png' , r'practice_katakana\\eminence_kata.png'])
word[1] = (["Philanthropy",r'practice_sound\\philanthropy.mp3', r'practice_image\\philanthropy.png', r'practice_text\\philanthropy_text.png', r'practice_imi\\philanthropy_imi.png',"Philanthropy.", r'practice_description\\philanthropy_des.png', r'practice_katakana\\philanthropy_kata.png'])
word[2] = (["Prestige",r'practice_sound\\prestige.mp3', r'practice_image\\prestige.png', r'practice_text\\prestige_text.png' ,'practice_imi\\prestige_imi.png',"Prestige.", r'practice_description\\prestige_des.png', r'practice_katakana\\prestige_kata.png'])
word[3] = (["Indiscretion",r'practice_sound\\indiscretion.mp3', r'practice_image\\indiscretion.png', r'practice_text\\indiscretion_text.png','practice_imi\\indiscretion_imi.png',"Indiscretion.", r'practice_description\\indiscretion_des.png', r'practice_katakana\\indiscretion_kata.png'])
word[4] = (["Duplicity",r'practice_sound\\duplicity.mp3', r'practice_image\\duplicity.png', r'practice_text\\duplicity_text.png','practice_imi\\duplicity_imi.png',"Duplicity.", r'practice_description\\duplicity_des.png', r'practice_katakana\\duplicity_kata.png'])
word[5] = (["Rampage",r'practice_sound\\rampage.mp3', r'practice_image\\rampage.png', r'practice_text\\rampage_text.png' ,'practice_imi\\rampage_imi.png',"Rampage.", r'practice_description\\rampage_des.png', r'practice_katakana\\rampage_kata.png'])
word[6] = (["Indolence",r'practice_sound\\indolence.mp3', r'practice_image\\indolence.png', r'practice_text\\indolence_text.png' ,'practice_imi\\indolence_imi.png',"Indolence.", r'practice_description\\indolence_des.png', r'practice_katakana\\indolence_kata.png'])
word[7] = (["Prodigy",r'practice_sound\\prodigy.mp3', r'practice_image\\prodigy.png', r'practice_text\\prodigy_text.png' ,'practice_imi\\prodigy_imi.png',"Prodigy.", r'practice_description\\prodigy_des.png', r'practice_katakana\\prodigy_kata.png'])
word[8] = (["Incentive",r'practice_sound\\incentive.mp3', r'practice_image\\incentive.png', r'practice_text\\incentive_text.png' ,'practice_imi\\incentive_imi.png', "Incentive.", r'practice_description\\incentive_des.png', r'practice_katakana\\incentive_kata.png'])
word[9] = (["Counterpart",r'practice_sound\\counterpart.mp3', r'practice_image\\counterpart.png', r'practice_text\\counterpart_text.png' ,'practice_imi\\counterpart_imi.png', "Counterpart.", r'practice_description\\Counterpart_des.png', r'practice_katakana\\counterpart_kata.png'])

#音声とイラストと意味とカタカナ(word[i][1], word[i][2], word[i][5], word[i][8]) 


word[10] = (["Candor",r'practice_sound\\candor.mp3', r'practice_image\\candor.png', r'practice_text\\candor_text.png', r'practice_imi\\candor_imi.png',"Candor.", r'practice_description\\candor_des.png' , r'practice_katakana\\candor_kata.png'])
word[11] = (["Demeanor",r'practice_sound\\demeanor.mp3', r'practice_image\\demeanor.png', r'practice_text\\demeanor_text.png', r'practice_imi\\demeanor_imi.png',"Demeanor.",  r'practice_description\\demeanor_des.png', r'practice_katakana\\demeanor_kata.png'])
word[12] = (["Recipient",r'practice_sound\\recipient.mp3', r'practice_image\\recipient.png', r'practice_text\\recipient_text.png' ,'practice_imi\\recipient_imi.png',"Recipient.", r'practice_description\\recipient_des.png', r'practice_katakana\\recipient_kata.png'])
word[13] = (["Fugitive",r'practice_sound\\fugitive.mp3', r'practice_image\\fugitive.png', r'practice_text\\fugitive_text.png','practice_imi\\fugitive_imi.png',"Fugitive.", r'practice_description\\fugitive_des.png', r'practice_katakana\\fugitive_kata.png'])
word[14] = (["Allegory",r'practice_sound\\allegory.mp3', r'practice_image\\allegory.png', r'practice_text\\allegory_text.png','practice_imi\\allegory_imi.png',"Allegory.", r'practice_description\\allegory_des.png', r'practice_katakana\\allegory_kata.png'])
word[15] = (["Affinity",r'practice_sound\\affinity.mp3', r'practice_image\\affinity.png', r'practice_text\\affinity_text.png' ,'practice_imi\\affinity_imi.png',"Affinity.",r'practice_description\\affinity_des.png', r'practice_katakana\\affinity_kata.png'])
word[16] = (["Preservation",r'practice_sound\\preservation.mp3', r'practice_image\\preservation.png', r'practice_text\\preservation_text.png' ,'practice_imi\\preservation_imi.png',"Preservation.",  r'practice_description\\preservation_des.png', r'practice_katakana\\preservation_kata.png'])
word[17] = (["Hindrance",r'practice_sound\\Hindrance.mp3', r'practice_image\\hindrance.png', r'practice_text\\hindrance_text.png' ,'practice_imi\\hindrance_imi.png',"Hindrance.", r'practice_description\\hindrance_des.png', r'practice_katakana\\hindrance_kata.png'])
word[18] = (["Breakthrough",r'practice_sound\\breakthrough.mp3', r'practice_image\\breakthrough.png', r'practice_text\\breakthrough_text.png' ,'practice_imi\\breakthrough_imi.png', "Breakthrough.", r'practice_description\\breakthrough_des.png', r'practice_katakana\\breakthrough_kata.png'])
word[19] = (["Adherent",r'practice_sound\\adherent.mp3', r'practice_image\\adherent.png', r'practice_text\\adherent_text.png' ,'practice_imi\\adherent_imi.png', "Adherent.", r'practice_description\\adherent_des.png', r'practice_katakana\\adherent_kata.png'])


#音声とイラストと意味とフォニックスとカタカナ (word[i][1], word[i][2], word[i][5], word[i][8], word[i][9])

word[20] = (["Gadget",r'practice_sound\\gadget.mp3', r'practice_image\\gadget.png', r'practice_text\\gadget_text.png','practice_imi\\gadget_imi.png',"Gadget.", r'practice_description\\gadget_des.png' , r'practice_katakana\\gadget_kata.png'])
word[21] = (["Footage",r'practice_sound\\footage.mp3', r'practice_image\\footage.png', r'practice_text\\footage_text.png', r'practice_imi\\footage_imi.png',"Footage.", r'practice_description\\footage_des.png', r'practice_katakana\\footage_kata.png'])
word[22] = (["Dissension",r'practice_sound\\dissension.mp3', r'practice_image\\dissension.png', r'practice_text\\dissension_text.png' ,'practice_imi\\dissension_imi.png',"Dissension.", r'practice_description\\dissension_des.png', r'practice_katakana\\dissension_kata.png'])
word[23] = (["Jeopardy",r'practice_sound\\jeopardy.mp3', r'practice_image\\jeopardy.png', r'practice_text\\jeopardy_text.png','practice_imi\\jeopardy_imi.png',"Jeopardy.", r'practice_description\\jeopardy_des.png', r'practice_katakana\\jeopardy_kata.png'])
word[24] = (["Flattery",r'practice_sound\\flattery.mp3', r'practice_image\\flattery.png', r'practice_text\\flattery_text.png','practice_imi\\flattery_imi.png',"Flattery.", r'practice_description\\flattery_des.png', r'practice_katakana\\flattery_kata.png'])
word[25] = (["Duration",r'practice_sound\\duration.mp3', r'practice_image\\duration.png', r'practice_text\\duration_text.png' ,'practice_imi\\duration_imi.png',"Duration.", r'practice_description\\duration_des.png', r'practice_katakana\\duration_kata.png'])
word[26] = (["Conglomerate",r'practice_sound\\conglomerate.mp3', r'practice_image\\conglomerate.png', r'practice_text\\conglomerate_text.png' ,'practice_imi\\conglomerate_imi.png',"Conglomerate.", r'practice_description\\conglomerate_des.png', r'practice_katakana\\conglomerate_kata.png'])
word[27] = (["Enforcement",r'practice_sound\\enforcement.mp3', r'practice_image\\enforcement.png', r'practice_text\\enforcement_text.png' ,'practice_imi\\enforcement_imi.png',"Enforcement.", r'practice_description\\enforcement_des.png', r'practice_katakana\\enforcement_kata.png'])
word[28] = (["Disguise",r'practice_sound\\disguise.mp3', r'practice_image\\disguise.png', r'practice_text\\disguise_text.png' ,'practice_imi\\disguise_imi.png', "Disguise.",  r'practice_description\\disguise_des.png', r'practice_katakana\\disguise_kata.png'])
word[29] = (["Prelude",r'practice_sound\\prelude.mp3', r'practice_image\\prelude.png', r'practice_text\\prelude_text.png' ,'practice_imi\\prelude_imi.png', "Prelude.", r'practice_description\\prelude_des.png', r'practice_katakana\\prelude_kata.png'])

#音声とイラストと意味と説明文とカタカナ (word[i][1], word[i][2], word[i][5], word[i][7], word[i][8]) 

word[30] = (["Transgression",r'practice_sound\\transgression.mp3', r'practice_image\\transgression.png', r'practice_text\\transgression_text.png', r'practice_imi\\transgression_imi.png',"Transgression.", r'practice_description\\transgression_des.png', r'practice_katakana\\transgression_kata.png'])
word[31] = (["Exhilaration",r'practice_sound\\exhilaration.mp3', r'practice_image\\exhilaration.png', r'practice_text\\exhilaration_text.png', r'practice_imi\\exhilaration_imi.png',"Exhilaration.",r'practice_description\\exhilaration_des.png', r'practice_katakana\\exhilaration_kata.png'])
word[32] = (["Culprit",r'practice_sound\\culprit.mp3', r'practice_image\\culprit.png', r'practice_text\\culprit_text.png' , r'practice_imi\\culprit_imi.png',"Culprit.", r'practice_description\\culprit_des.png', r'practice_katakana\\culprit_kata.png' ])
word[33] = (["Phase",r'practice_sound\\phase.mp3', r'practice_image\\phase.png', r'practice_text\\phase_text.png', r'practice_imi\\phase_imi.png',"Phase.", r'practice_description\\phase_des.png', r'practice_katakana\\phase_kata.png'])
word[34] = (["Gravity",r'practice_sound\\gravity.mp3', r'practice_image\\gravity.png', r'practice_text\\gravity_text.png', r'practice_imi\\gravity_imi.png',"Gravity.", r'practice_description\\gravity_des.png', r'practice_katakana\\gravity_kata.png'])
word[35] = (["Specimen",r'practice_sound\\specimen.mp3', r'practice_image\\specimen.png', r'practice_text\\specimen_text.png' , r'practice_imi\\specimen_imi.png',"Specimen.", r'practice_description\\specimen_des.png', r'practice_katakana\\specimen_kata.png'])
word[36] = (["Endowment",r'practice_sound\\endowment.mp3', r'practice_image\\endowment.png', r'practice_text\\endowment_text.png' , r'practice_imi\\endowment_imi.png',"Endowment.",  r'practice_description\\endowment_des.png', r'practice_katakana\\endowment_kata.png'])
word[37] = (["Caliber",r'practice_sound\\caliber.mp3', r'practice_image\\caliber.png', r'practice_text\\caliber_text.png' , r'practice_imi\\caliber_imi.png',"Caliber.", r'practice_description\\caliber_des.png', r'practice_katakana\\caliber_kata.png'])
word[38] = (["Conviviality",r'practice_sound\\conviviality.mp3', r'practice_image\\conviviality.png', r'practice_text\\conviviality_text.png' , r'practice_imi\\conviviality_imi.png', "Conviviality.", r'practice_description\\conviviality_des.png', r'practice_katakana\\conviviality_kata.png'])
word[39] = (["Myriad",r'practice_sound\\myriad.mp3', r'practice_image\\myriad.png', r'practice_text\\myriad_text.png' , r'practice_imi\\myriad_imi.png', "Myriad.", r'practice_description\\myriad_des.png', r'practice_katakana\\myriad_kata.png'])

#音声と意味と説明文とカタカナ(word[i][1], word[i][5]

word[40] = (["Momentum",r'practice_sound\\momentum.mp3', r'practice_image\\momentum.png', r'practice_text\\momentum_text.png', r'practice_imi\\momentum_imi.png',"Momentum.", r'practice_description\\momentum_des.png', r'practice_katakana\\momentum_kata.png'])
word[41] = (["Perspective",r'practice_sound\\perspective.mp3', r'practice_image\\perspective.png', r'practice_text\\perspective_text.png', r'practice_imi\\perspective_imi.png',"Perspective.", r'practice_description\\perspective_des.png', r'practice_katakana\\perspective_kata.png'])
word[42] = (["Zeal",r'practice_sound\\zeal.mp3', r'practice_image\\zeal.png', r'practice_text\\zeal_text.png' , r'practice_imi\\zeal_imi.png',"Zeal.", r'practice_description\\zeal_des.png', r'practice_katakana\\zeal_kata.png'])
word[43] = (["Aptitude",r'practice_sound\\aptitude.mp3', r'practice_image\\aptitude.png', r'practice_text\\aptitude_text.png', r'practice_imi\\aptitude_imi.png',"Aptitude.", r'practice_description\\aptitude_des.png', r'practice_katakana\\aptitude_kata.png'])
word[44] = (["Antipathy",r'practice_sound\\antipathy.mp3', r'practice_image\\antipathy.png', r'practice_text\\antipathy_text.png', r'practice_imi\\antipathy_imi.png',"Antipathy.", r'practice_description\\antipathy_des.png', r'practice_katakana\\antipathy_kata.png'])
word[45] = (["Gratuity",r'practice_sound\\gratuity.mp3', r'practice_image\\gratuity.png', r'practice_text\\gratuity_text.png' , r'practice_imi\\gratuity_imi.png',"Gratuity.", r'practice_description\\gratuity_des.png', r'practice_katakana\\gratuity_kata.png'])
word[46] = (["Allure",r'practice_sound\\allure.mp3', r'practice_image\\allure.png', r'practice_text\\allure_text.png' , r'practice_imi\\allure_imi.png',"Allure.", r'practice_description\\allure_des.png', r'practice_katakana\\allure_kata.png'])
word[47] = (["Cipher",r'practice_sound\\cipher.mp3', r'practice_image\\cipher.png', r'practice_text\\cipher_text.png' , r'practice_imi\\cipher_imi.png',"Cipher.",r'practice_description\\cipher_des.png', r'practice_katakana\\cipher_kata.png'])
word[48] = (["Defiance",r'practice_sound\\defiance.mp3', r'practice_image\\defiance.png', r'practice_text\\defiance_text.png' , r'practice_imi\\defiance_imi.png', "Defiance.", r'practice_description\\defiance_des.png', r'practice_katakana\\defiance_kata.png'])
word[49] = (["Longevity",r'practice_sound\\longevity.mp3', r'practice_image\\longevity.png', r'practice_text\\longivity_text.png' , r'practice_imi\\longevity_imi.png', "Longevity.", r'practice_description\\longevity_des.png', r'practice_katakana\\longevity_kata.png'])



word[50] = (["Influx",r'practice_sound\\influx.mp3', r'practice_image\\influx.png', r'practice_text\\influx_text.png', r'practice_imi\\influx_imi.png',"Influx.", r'practice_description\\influx_des.png', r'practice_katakana\\influx_kata.png'])
word[51] = (["Mentor",r'practice_sound\\mentor.mp3', r'practice_image\\mentor.png', r'practice_text\\mentor_text.png', r'practice_imi\\mentor_imi.png',"Mentor.", r'practice_description\\mentor_des.png', r'practice_katakana\\mentor_kata.png'])
word[52] = (["Perception",r'practice_sound\\perception.mp3', r'practice_image\\perception.png', r'practice_text\\perception_text.png' , r'practice_imi\\perception_imi.png',"Perception.", r'practice_description\\perception_des.png', r'practice_katakana\\perception_kata.png'])
word[53] = (["Verdict",r'practice_sound\\verdict.mp3', r'practice_image\\verdict.png', r'practice_text\\verdict_text.png', r'practice_imi\\verdict_imi.png',"Verdict.", r'practice_description\\verdict_des.png', r'practice_katakana\\verdict_kata.png'])
word[54] = (["Proponent",r'practice_sound\\proponent.mp3', r'practice_image\\proponent.png', r'practice_text\\proponent_text.png', r'practice_imi\\proponent_imi.png',"Proponent.", r'practice_description\\proponent_des.png', r'practice_katakana\\proponent_kata.png'])
word[55] = (["Dexterity",r'practice_sound\\dexterity.mp3', r'practice_image\\dexterity.png', r'practice_text\\dexterity_text.png' , r'practice_imi\\dexterity_imi.png',"Dexterity.", r'practice_description\\dexterity_des.png', r'practice_katakana\\dexterity_kata.png'])
word[56] = (["Prosecutor",r'practice_sound\\prosecutor.mp3', r'practice_image\\prosecutor.png', r'practice_text\\prosecutor_text.png' , r'practice_imi\\prosecutor_imi.png',"Prosecutor.", r'practice_description\\prosecutor_des.png', r'practice_katakana\\prosecutor_kata.png'])
word[57] = (["Posture",r'practice_sound\\posture.mp3', r'practice_image\\posture.png', r'practice_text\\posture_text.png' , r'practice_imi\\posture_imi.png',"Posture.", r'practice_description\\posture_des.png', r'practice_katakana\\posture_kata.png'])
word[58] = (["Blunder",r'practice_sound\\blunder.mp3', r'practice_image\\blunder.png', r'practice_text\\blunder_text.png' , r'practice_imi\\blunder_imi.png', "Blunder.", r'practice_description\\blunder_des.png', r'practice_katakana\\blunder_kata.png'])
word[59] = (["Proximity",r'practice_sound\\proximity.mp3', r'practice_image\\proximity.png', r'practice_text\\proximity_text.png' , r'practice_imi\\proximity_imi.png', "Proximity.", r'practice_description\\proximity_des.png', r'practice_katakana\\proximity_kata.png'])



word[60] = (["Jinx",r'practice_sound\\jinx.mp3', r'practice_image\\jinx.png', r'practice_text\\jinx_text.png', r'practice_imi\\jinx_imi.png',"Jinx.", r'practice_description\\jinx_des.png', r'practice_katakana\\jinx_kata.png'])
word[61] = (["Forgery",r'practice_sound\\forgery.mp3', r'practice_image\\forgery.png', r'practice_text\\forgery_text.png', r'practice_imi\\forgery_imi.png',"Forgery.",  r'practice_description\\forgery_des.png', r'practice_katakana\\forgery_kata.png'])
word[62] = (["Extinction",r'practice_sound\\extinction.mp3', r'practice_image\\extinction.png', r'practice_text\\extinction_text.png' , r'practice_imi\\extinction_imi.png',"Extinction.", r'practice_description\\extinction_des.png', r'practice_katakana\\extinction_kata.png'])
word[63] = (["Consort",r'practice_sound\\consort.mp3', r'practice_image\\consort.png', r'practice_text\\consort_text.png', r'practice_imi\\consort_imi.png',"Consort.", r'practice_description\\consort_des.png', r'practice_katakana\\consort_kata.png'])
word[64] = (["Ordeal",r'practice_sound\\ordeal.mp3', r'practice_image\\ordeal.png', r'practice_text\\ordeal_text.png', r'practice_imi\\ordeal_imi.png',"Ordeal.", r'practice_description\\ordeal_des.png', r'practice_katakana\\ordeal_kata.png'])
word[65] = (["Fidelity",r'practice_sound\\fidelity.mp3', r'practice_image\\fidelity.png', r'practice_text\\fidelity_text.png' , r'practice_imi\\fidelity_imi.png',"Fidelity.",r'practice_description\\fidelity_des.png', r'practice_katakana\\fidelity_kata.png'])
word[66] = (["Diversity",r'practice_sound\\diversity.mp3', r'practice_image\\diversity.png', r'practice_text\\diversity_text.png' , r'practice_imi\\diversity_imi.png',"Diversity.", r'practice_description\\diversity_des.png', r'practice_katakana\\diversity_kata.png'])
word[67] = (["Backlash",r'practice_sound\\backlash.mp3', r'practice_image\\backlash.png', r'practice_text\\backlash_text.png' , r'practice_imi\\backlash_imi.png',"Backlash.", r'practice_description\\backlash_des.png', r'practice_katakana\\backlash_kata.png'])
word[68] = (["Agility",r'practice_sound\\agility.mp3', r'practice_image\\agility.png', r'practice_text\\agility_text.png' , r'practice_imi\\agility_imi.png', "Agility.", r'practice_description\\agility_des.png', r'practice_katakana\\agility_kata.png'])
word[69] = (["Novice",r'practice_sound\\novice.mp3', r'practice_image\\novice.png', r'practice_text\\novice_text.png' , r'practice_imi\\novice_imi.png', "Novice.",r'practice_description\\novice_des.png', r'practice_katakana\\novice_kata.png'])

#スクリーンサイズ取得
sc_w = GetSystemMetrics(0)
sc_h = GetSystemMetrics(1)

#words = np.array(word)


#ran = random.randint(0,word_num)

def disp_img(img,banana):

	print(img[0]) 

	if banana == 1:	# disp_image_kata_dscr
		#イラスト画像の表示 
		#LinerImg = cv2.resize(cv2.imread(img[2]),(700, 700))
		LinerImg = cv2.imread(img[2])
		cv2.imshow('LinerImg', LinerImg)
		cv2.moveWindow('LinerImg', 0, 0)
		
		#意味画像の表示       
		#LinerImg2 = cv2.resize(cv2.imread(img[4]), (512, 200))
		LinerImg2 = cv2.imread(img[4])    
		cv2.imshow('LinerImg2',LinerImg2)     
		cv2.moveWindow('LinerImg2',0,480)
		
		LinerImg3 = cv2.imread(img[6])    
		cv2.imshow('LinerImg3',LinerImg3)     
		cv2.moveWindow('LinerImg3',0,680)
		
		LinerImg4 = cv2.imread(img[7])    
		cv2.imshow('LinerImg4',LinerImg4)     
		cv2.moveWindow('LinerImg4',0,880)
		
	elif banana == 2:	#disp_image_kata(img):
		#イラスト画像の表示 
		#LinerImg = cv2.resize(cv2.imread(img[2]),(700, 700))
		LinerImg5 = cv2.imread(img[2])
		cv2.imshow('LinerImg5', LinerImg5)
		cv2.moveWindow('LinerImg5', 0, 0)

		#意味画像の表示       
		#LinerImg2 = cv2.resize(cv2.imread(img[4]), (512, 200))
		LinerImg6 = cv2.imread(img[4])    
		cv2.imshow('LinerImg6',LinerImg6)     
		cv2.moveWindow('LinerImg6',0,480)
		
		
		LinerImg7 = cv2.imread(img[7])    
		cv2.imshow('LinerImg7',LinerImg7)     
		cv2.moveWindow('LinerImg7',0,680)
		
	elif banana == 3:	# disp_image_dcsr(img):
		#イラスト画像の表示 
		#LinerImg = cv2.resize(cv2.imread(img[2]),(700, 700))
		LinerImg8 = cv2.imread(img[2])
		cv2.imshow('LinerImg8', LinerImg8)
		cv2.moveWindow('LinerImg8', 0, 0)

		#意味画像の表示       
		#LinerImg2 = cv2.resize(cv2.imread(img[4]), (512, 200))
		LinerImg9 = cv2.imread(img[4])    
		cv2.imshow('LinerImg9',LinerImg9)     
		cv2.moveWindow('LinerImg9',0,480)
		
		LinerImg10 = cv2.imread(img[6])    
		cv2.imshow('LinerImg10',LinerImg10)     
		cv2.moveWindow('LinerImg10',0,680)

	elif banana == 4:	#disp_kata_dcsr(img):
		#意味画像の表示       
		#LinerImg2 = cv2.resize(cv2.imread(img[4]), (512, 200))
		LinerImg11 = cv2.imread(img[4])    
		cv2.imshow('LinerImg11',LinerImg11)     
		cv2.moveWindow('LinerImg11',0,0)
		
		
		LinerImg12 = cv2.imread(img[6])    
		cv2.imshow('LinerImg12',LinerImg12)     
		cv2.moveWindow('LinerImg12',0,200)
		
		LinerImg13 = cv2.imread(img[7])    
		cv2.imshow('LinerImg13',LinerImg13)     
		cv2.moveWindow('LinerImg13',0,400)
		
	elif banana == 5:	#disp_kata(img):
		#意味画像の表示       
		#LinerImg13 = cv2.resize(cv2.imread(img[4]), (512, 200))
		LinerImg14 = cv2.imread(img[4])    
		cv2.imshow('LinerImg14',LinerImg14)     
		cv2.moveWindow('LinerImg14',0,0)

		
		LinerImg15 = cv2.imread(img[7])    
		cv2.imshow('LinerImg15',LinerImg15)     
		cv2.moveWindow('LinerImg15',0,200)

	elif banana == 6:	#disp_dcsr(img):
	
		#意味画像の表示       
		#LinerImg = cv2.resize(cv2.imread(img[4]), (512, 200))
		LinerImg16 = cv2.imread(img[4])    
		cv2.imshow('LinerImg16',LinerImg16)     
		cv2.moveWindow('LinerImg16',0,0)

		LinerImg17 = cv2.imread(img[6])    
		cv2.imshow('LinerImg17',LinerImg17)     
		cv2.moveWindow('LinerImg17',0,200)


	elif banana == 7:
		#イラスト画像の表示 
		#LinerImg = cv2.resize(cvw2.imread(img[2]),(700, 700))
		LinerImg18 = cv2.imread(img[2])
		cv2.imshow('LinerImg18', LinerImg18)
		cv2.moveWindow('LinerImg18', 0, 0)

		#LinerImg2 = cv2.resize(cv2.imread(img[4]), (512, 200))
		LinerImg19 = cv2.imread(img[4])    
		cv2.imshow('LinerImg19',LinerImg19)     
		cv2.moveWindow('LinerImg19',0,480)
		
	elif banana == 8:
		TextImg = cv2.imread(img[3])
		cv2.imshow('TextImg', TextImg)
		cv2.moveWindow('TextImg', 0, 0)
	
	
	pygame.mixer.init()
	pygame.mixer.music.load(img[1])
	mp3_length = mp3(img[1]).info.length
	pygame.mixer.music.play(1) 
	time.sleep(mp3_length + 0.25)  
	pygame.mixer.music.stop()
	cv2.waitKey(0)


	#word[] = [単語0、音声1、イラスト2、テキスト画像3、意味4、認識単語5、説明6、カタカナ7]
	
	
#文字認識を行うコード

def word_ocr():
	tools = pyocr.get_available_tools()
	if len(tools) == 0:
		print("No OCR tool found")
		sys.exit(1)
	tool = tools[0]
	last_txt = ""
	langs = tool.get_available_languages()
	lang = langs[0]
	capture = cv2.VideoCapture(0)
	last_txt = ""
	while True:
		ret, frame = capture.read()
		
		orgHeight, orgWidth = frame.shape[:2]
		size = (500, 500)
		glay = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
		image = cv2.resize(glay, size)
		
		cv2.moveWindow('Capture',-1920,480)
		edframe = frame
		t = time.time()

		#画像の中にある文字を"txt"の中に代入
		txt = tool.image_to_string(
			Image.fromarray(image),
			lang="eng",
			builder=pyocr.builders.TextBuilder(tesseract_layout=6)
		) 
		for i in range(word_num):
			if(txt == word[i][0]):
				return i
		cv2.imshow("Capture", image)
		if cv2.waitKey(33) >= 0:
			break



#音声認識コード

def sound_in():
	speech_key, service_region = "57b79b39b99a4e67889c7d7beae49f20", "westus"
	speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
	speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config)
	print("Say something...")
	result = speech_recognizer.recognize_once()

	if result.reason == speechsdk.ResultReason.RecognizedSpeech:
	    print("Recognized: {}".format(result.text))	    
	    
	elif result.reason == speechsdk.ResultReason.NoMatch:
	    print("No speech could be recognized: {}".format(result.no_match_details))
	    
	elif result.reason == speechsdk.ResultReason.Canceled:
	    cancellation_details = result.cancellation_details
	    print("Speech Recognition canceled: {}".format(cancellation_details.reason))
	    
	    if cancellation_details.reason == speechsdk.CancellationReason.Error:
	        print("Error details: {}".format(cancellation_details.error_details))
	        
	#"te1"という変数に音声認識したテキストを格納
	return result.text





def jud_text(tex,numbe,new_numbe):

	if(tex == word[numbe][5]):   
		score[numbe] += 1
		print('あなたは{}回目に正解しました。'.format(score[numbe]))
		new_score[new_numbe] = score[numbe]
		return 0
	elif(tex != word[numbe][5]):
		score[numbe] += 1
		print("ちゃんと発音してね")
		while True:
			if msvcrt.kbhit():
				kb = msvcrt.getch()
				if kb.decode() == 'a':
					pygame.mixer.init()
					pygame.mixer.music.load(word[numbe][1])
					mp3_length = mp3(word[numbe][1]).info.length 
					pygame.mixer.music.play(1) 
					time.sleep(mp3_length + 0.25) 
					pygame.mixer.music.stop()
				elif kb.decode() == 'b':
					return 1
				elif kb.decode() == 'c':
					score[numbe] = 0
					new_score[new_numbe] = score[numbe]
					return 0



def fin_scr(thescore):

	for i in range(2):
		if(thescore[i] == 0):
			print(new_word[i][0]+'は音声認識できませんでした')
		else:
			print(new_word[i][0]+'は{}回目に音声認識できました'.format(thescore[i]))


def jud_test_text(tex,numbe):
	if(tex == new_word[numbe][5]):   
		test_score[numbe] += 1
		print('あなたは{}回目に正解しました。'.format(test_score[numbe]))
		return 0
	elif(tex != new_word[numbe][5]):
		test_score[numbe] += 1
		print("ちゃんと発音してね")
		while True:
			if msvcrt.kbhit():
				kb = msvcrt.getch()
				if kb.decode() == 'a':
					pygame.mixer.init()
					pygame.mixer.music.load(new_word[numbe][1])
					mp3_length = mp3(new_word[numbe][1]).info.length 
					pygame.mixer.music.play(1) 
					time.sleep(mp3_length + 0.25) 
					pygame.mixer.music.stop()
				elif kb.decode() == 'b':
					return 1
				elif kb.decode() == 'c':
					test_score[numbe] = 0
					return 0


def main():
	l = [1,2,3,4,5,6,7]
	#m = [1,2,3,4,5,6,7,8,9,10]
	m = [1,2]
	
	nnumber = 0
	#rand_sort()
	x = random.sample(l,7)
	#y = random.sample(m,10)
	y = random.sample(m,2)
	print(x)
	print(y)
	

	for k in range(0,7):	
		
		for i in range(0,2):
			#ran = random.randint(0,word_num+1)
			#cv2.imshow('text',cv2.imread(new_word[i][3]))	
			number = word_ocr()
			print(number)
			#disp_img(word[number],x[k])
			disp_img(word[number],1)
			new_word[i] = word[number]
			nw = new_word[i]
			nw.append(word[number])
			jud_fin = 1
			while jud_fin == 1:
				sd_text = sound_in()
				jud_fin = jud_text(sd_text,number,nnumber)
			cv2.destroyAllWindows()
			nnumber += 1
		fin_scr(new_score)
		
		start = input('Enterを押すとテストが開始します')
		
		for j in range(0, 2):
			number = y[j]
			disp_img(new_word[number-1],8)
			jud_fin = 1
			while jud_fin == 1:
				sd_text = sound_in()
				jud_fin = jud_test_text(sd_text,number-1)
			cv2.destroyAllWindows()
		fin_scr(test_score)
		
		wait = input('実験者の合図があったらEnterボタンを押してください')
		
		new_score.clear()
		new_word.clear()
		test_score.clear()
		print(new_score)

		#test_score = [0]*10
		
		print(test_score)
	print("終了です。お疲れさまでした。")
if __name__ == "__main__":
		main()

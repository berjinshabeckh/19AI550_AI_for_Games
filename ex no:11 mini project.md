# Ex.No: 11  Mini Project 
### DATE: 7.11.2024
### REGISTER NUMBER : 212222240018
### AIM: 
To write a python program to simulate the game using Reinforcement Learning to create an adaptive and challenging enemy AI. The enemies (e.g., pirate NPCs) will make decisions based on a
reward system that reinforces actions leading them closer to defeating or challenging the player. For simplicity, we’ll use Q-Learning—one of the more accessible RL algorithms.

### Algorithm:
->The environment here is the game map, where the agent (enemy pirate) has to navigate platforms, avoid obstacles, and pursue the player.<br>
->Actions are the possible moves the enemy pirate can make.<br>
->Reward the pirate NPC for actions that bring it closer to desired outcomes, such as challenging or defeating the player.<br>
->Q-Learning uses a Q-Table to map State-Action pairs to rewards. It learns over time which actions yield the highest cumulative rewards in specific states.<br>
->Exploration vs. Exploitation: Initially, let the enemy explore actions randomly (exploration) to gather data. Gradually 
reduce randomness (exploit learned strategies) as it learns which actions maximize rewards.<br>

### Program:

main.py
```
from settings import * 
from level import Level
from pytmx.util_pygame import load_pygame
from os.path import join
from support import * 
from data import Data
from debug import debug
from ui import UI
from overworld import Overworld

class Game:
	def __init__(self):
		pygame.init()
		self.display_surface = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT))
		pygame.display.set_caption('Super Pirate')
		self.clock = pygame.time.Clock()
		self.import_assets()

		self.ui = UI(self.font, self.ui_frames)
		self.data = Data(self.ui)
		self.tmx_maps = {
			0: load_pygame(join('..', 'data', 'levels', 'omni.tmx')),
			1: load_pygame(join('..', 'data', 'levels', '1.tmx')),
			2: load_pygame(join('..', 'data', 'levels', '2.tmx')),
			}
		self.tmx_overworld = load_pygame(join('..', 'data', 'overworld', 'overworld.tmx'))
		self.current_stage = Level(self.tmx_maps[self.data.current_level], self.level_frames, self.data, self.switch_stage)

	def switch_stage(self, target, unlock = 0):
		if target == 'level':
			self.current_stage = Level(self.tmx_maps[self.data.current_level], self.level_frames, self.data, self.switch_stage)
			
		else: # overworld 
			if unlock > 0:
				self.data.unlocked_level = 6
			else:
				self.data.health -= 1
			self.current_stage = Overworld(self.tmx_overworld, self.data, self.overworld_frames, self.switch_stage)

	def import_assets(self):
		self.level_frames = {
			'flag': import_folder('..', 'graphics', 'level', 'flag'),
			'saw': import_folder('..', 'graphics', 'enemies', 'saw', 'animation'),
			'floor_spike': import_folder('..', 'graphics','enemies', 'floor_spikes'),
			'palms': import_sub_folders('..', 'graphics', 'level', 'palms'),
			'candle': import_folder('..', 'graphics','level', 'candle'),
			'window': import_folder('..', 'graphics','level', 'window'),
			'big_chain': import_folder('..', 'graphics','level', 'big_chains'),
			'small_chain': import_folder('..', 'graphics','level', 'small_chains'),
			'candle_light': import_folder('..', 'graphics','level', 'candle light'),
			'player': import_sub_folders('..', 'graphics','player'),
			'saw': import_folder('..', 'graphics', 'enemies', 'saw', 'animation'),
			'saw_chain': import_image('..',  'graphics', 'enemies', 'saw', 'saw_chain'),
			'helicopter': import_folder('..', 'graphics', 'level', 'helicopter'),
			'boat': import_folder('..',  'graphics', 'objects', 'boat'),
			'spike': import_image('..',  'graphics', 'enemies', 'spike_ball', 'Spiked Ball'),
			'spike_chain': import_image('..',  'graphics', 'enemies', 'spike_ball', 'spiked_chain'),
			'tooth': import_folder('..', 'graphics','enemies', 'tooth', 'run'),
			'shell': import_sub_folders('..', 'graphics','enemies', 'shell'),
			'pearl': import_image('..',  'graphics', 'enemies', 'bullets', 'pearl'),
			'items': import_sub_folders('..', 'graphics', 'items'),
			'particle': import_folder('..', 'graphics', 'effects', 'particle'),
			'water_top': import_folder('..', 'graphics', 'level', 'water', 'top'),
			'water_body': import_image('..', 'graphics', 'level', 'water', 'body'),
			'bg_tiles': import_folder_dict('..', 'graphics', 'level', 'bg', 'tiles'),
			'cloud_small': import_folder('..', 'graphics','level', 'clouds', 'small'),
			'cloud_large': import_image('..', 'graphics','level', 'clouds', 'large_cloud'),
		}
		self.font = pygame.font.Font(join('..', 'graphics', 'ui', 'runescape_uf.ttf'), 40)
		self.ui_frames = {
			'heart': import_folder('..', 'graphics', 'ui', 'heart'), 
			'coin':import_image('..', 'graphics', 'ui', 'coin')
		}
		self.overworld_frames = {
			'palms': import_folder('..', 'graphics', 'overworld', 'palm'),
			'water': import_folder('..', 'graphics', 'overworld', 'water'),
			'path': import_folder_dict('..', 'graphics', 'overworld', 'path'),
			'icon': import_sub_folders('..', 'graphics', 'overworld', 'icon'),
		}

	def check_game_over(self):
		if self.data.health <= 0:
			pygame.quit()
			sys.exit()

	def run(self):
		while True:
			dt = self.clock.tick() / 1000
			for event in pygame.event.get():
				if event.type == pygame.QUIT:
					pygame.quit()
					sys.exit()

			self.check_game_over()
			self.current_stage.run(dt)
			self.ui.update(dt)
			
			pygame.display.update()

if __name__ == '__main__':
	game = Game()
	game.run()
```

### Output:
![Screenshot 2024-11-07 221451](https://github.com/user-attachments/assets/d84d2132-ec74-440a-98fc-ab735200170f)
![Screenshot 2024-11-07 221615](https://github.com/user-attachments/assets/c056c7f6-c721-45a3-b11e-bbde77b16b0a)
![Screenshot 2024-11-07 221715](https://github.com/user-attachments/assets/461d4c01-9182-499a-9268-9ce416564d9f)
![Screenshot 2024-11-07 221735](https://github.com/user-attachments/assets/011417e3-3b4f-4630-a4d2-ae747f440418)


### Result:
Thus the simple  game was implemented using pygame.

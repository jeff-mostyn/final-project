# Final Project!

This is it! The culmination of your procedural graphics experience this semester. For your final project, we'd like to give you the time and space to explore a topic of your choosing. You may choose any topic you please, so long as you vet the topic and scope with an instructor or TA. We've provided some suggestions below. The scope of your project should be roughly 1.5 homework assignments). To help structure your time, we're breaking down the project into 4 milestones:

## Project planning: Design Doc (due 11/6)
Before submitting your first milestone, _you must get your project idea and scope approved by Rachel, Adam or a TA._

### Design Doc
Start off by forking this repository. In your README, write a design doc to outline your project goals and implementation plan. It must include the following sections:

#### Introduction
For a couple of years on and off, I have been using free time to work on a racing game in Unreal Engine with some friends of mine. However, as a small team we have been often plagued by bottlenecks, especially when it comes to art assets. Since our game takes place largely in city environments, a number of assets are needed to create a believable, unique space that does not feel empty. When we began our assignment to make a basic building generator, I thought that a tool like that, applied to our game, could save us a significant amount of time. As I grew in confidence and understanding of Houdini, I came to decide that I wanted to create a such a tool that would be usable for us and teams like ours.

#### Goal
The end goal of this project is to create a plugin tool for Unreal Engine that artists can use to speed up their level environment population workflow by generating varied buildings from parameter sets rather than making them from scratch. 

#### Inspiration/reference:
- I am using the previous [Building Generation Homework](https://github.com/jeff-mostyn/hw03-buildings) as a baseline for this project.
- Said homework itself is based on a [tutorial video](https://www.youtube.com/watch?v=uIe97023sDk&t=979s&ab_channel=SimonHoudini) on using Houdini to make a building generator. Both the original homework and this extension will further expand upon the concept presented here.

#### Specification:
- Houdini HDA that generates houses/buildings procedurally from a set of inputs. Breadth of features that can be added in the generation will depend on available time, but will at the very least include building dimensions, doors, windows, decks/balconies, and edge accents.
- The HDA will have "default" assets for features such as windows, doors, accents, etc.
- A direct Unreal Engine interface/plugin that communicates with the Houdini HDA
- Assuming it is technologically possible, I want the Unreal Engine interface to allow for the artist using it to select custom assets from their computer to use for windows, doors, and the like.
- The output of the HDA will create a building directly in Unreal Engine, which can have Unreal Engine materials 

#### Techniques:
- The bulk of the procedural work will be done in Houdini. There are presently no specific algorithms that are being investigated for use.
- I will be making use of [Houdini-Unreal Engine integration](https://www.sidefx.com/docs/unreal16.5/index.html)

#### Design:
![image](https://github.com/user-attachments/assets/b1c0afe5-9bc1-4fc3-89e7-984ce11e61cc)


#### Timeline:
- Week 1
  - Integrate basic generator HDA into Unreal Engine
  - Establish workflow for building generation in-engine
  - Determine if material application onto generated building works as desired out-of-the-box - does each individual piece accept its own material? Do materials "slots" need to be made in advance in Houdini?
  - If materials need extra work to function as intended, perform that work
 
- Week 2
  - Determine viability of custom asset importing through UE5
    - If it is not possible, modify HDA to accept non-Houdini assets via Houdini, and select them via a parameter in the interface
  - Move to additional feature work on building generator. Examples:
    - Deck roofs (tiled/shingled)
    - Increased control of ratio of windows to doors, especially on first floor
    - Added variation for corner pillars
    - Parameterization of deck/balcony posts
   
- Final
  - Continued polish on generator
  - Create sample assets in Maya (meshes) and Unreal Engine (materials)
  - Make demo video

## Milestone 1

### Progress
- Successfully attached original building generator HDA to an existing Unreal Engine project
- Fixed issues within the generator which prevented it from working.
  - The "default" window, door, and balcony assets were assembled in separate geometry nodes. These had to be remade into subnets within the HDA
- Produced output from the generator within UE5, equivalent to that which is visible within Houdini as output of the original HDA

![image](https://github.com/user-attachments/assets/8e7d13c7-46df-4355-98c7-ff0eefc17584)
  
- Researched and learned how to reference UE5 materials directly within an attached HDA. Applied this to my project.
- Figured out how to use UE5 materials as inputs to the HDA, enabling swapping of materials without leaving the engine.

![](https://github.com/jeff-mostyn/final-project/blob/main/Images/changingMatsGif.gif)

- Created basic set of material parameters, and updated the HDA node network to enable all parts to have their materials driven by those parameters
- Since I was ahead of schedule, I looked towards my week 2 goals, and figured out how to pass custom geometry as parameters into the HDA
- Updated the HDA node network to support parameterized geometry for windows and doors

![image](https://github.com/user-attachments/assets/42b9acc1-6aee-45b5-9149-b54e4f02c20e)
(The doors are currently tiny, as explained below)

### Shortfalls
- Sizing of parameter geometry is currently causing issues. Custom doors are coming in very small. I think I will again need to update the HDA such that scales/transforms of custom geometry can be manipulated from UE5 interface.


## Milestone 2

### Progress
- Finalized imported geometry procedure. Added transformation manipulation to adjust position relative to default, as well as scale. Custom assets fed to Houdini via UE5 maintain their UVs and so can have custom materials applied
- Added support for custom windows and deck rail posts
- Augmented functionality of building generator
  - Added procedural tile rooves over the uppermost full-length decks
  - Added support pillars on wraparound decks, if the next floor up also has a deck, or if it is the highest floor
  - Added pillar variety to include a stone pattern
  - Fixed outstanding issue where full-length balconies did not have rails on the ends (perpendicular to wall)

- Overview image of generated houses with Milestone 2 progress
![image](https://github.com/user-attachments/assets/2cc03080-9b11-47f6-8320-0d4bc6a198f4)


### Shortfalls
- Asset placement logic nees to be updated. All doors on floors that have even 1 full-length balcony appear without individual balconies. This can lead to doors to nowhere. Additionally, having better control over relative quantity of doors and windows would be ideal.


## Final Submission

### Post-Mortem

This project, despite initial expectations, went fairly smoothly. There were minimal problems encountered, and much of what I feared would involve lengthy implementation time was more or less straightforward. The crowning example of this was in importing mesh assets into the Houdini HDA via Unreal Engine. As it turns out, on the HDA Type Properties, you are able to arbitrarily increase the number of inputs, and even give each one a custom name, which appear in the Unreal Engine interface natively (pictured below)

![image](https://github.com/user-attachments/assets/14b1002a-ba0f-4a44-b93f-9a78226e3299)

Ironically, one of the larger hurdles I have run into has been in preparing my final submission for this project. Up to this point, I had been experimenting with the HDA in a separate Unreal Engine project which I had already configured. However, in the process of setting up the project that can be found in this repository, I found an error where attempting to save the level which contains an HDA instance causes the engine to cease responding, requiring Task Manager to close it down. It seems that otherwise everything checks out, I'm able to interact with the HDA, make adjustments, but the act of saving caused a problem. If a solution is found between the time of writing this and submitting the project, I will note as much here. But this aside, there were no real problems to speak of, meaning that I never really had to stray from my original plan, excepting scenarios where a certain item took slightly more or less time as compared to my initial expectations.

Beyond that, I am fairly happy with what I have been able to create here. The additions I made within the generator to further come into line with the architectural style I was pursuing definitely help sell that feeling. Improving the floor-length decks, setting tiled rooves overtop of them, and adding various support structures to the decks (posts and below-deck struts) increase the "realism" / "authenticity" of the buildings. Beyond that, being able to simply create new doors and windows and texture them freely as though they were standalone items gives miles more flexibility to this tool. While I would have liked to have been able to implement other swappable assets and ornamentation given more time, as a baseline the product I've created met all the goals I really wanted it to. It is extremely simple to use and update, which was the primary objective for me, as I think in a cooperative scenario I could easily hand this off to an artist with a brief explanation, and they should have little issue making use of the tool.


### Live Demo

[Youtube live demo](https://youtu.be/PY_T6Kcm6AQ)


### Example Images

![image](https://github.com/user-attachments/assets/3adf7d13-4267-449a-ae8d-71d8d233c2ea)

![image](https://github.com/user-attachments/assets/a195900d-3ec5-49df-ab8c-95406b29ba53)

![image](https://github.com/user-attachments/assets/a07243e0-d01c-4942-8c3d-c018236bdb83)

## Demo information

This repoository contains a Unreal Engine 5.3 project, that should come pre-prepared with the Houdini Engine plugin (though it will need to compile before running). 

To test the HDA

1. Go to the level `final-project\Unreal Project\BuildingGenDemo\Content\DemoFiles\Demo`, and interact with the HDA actor in the level.

2. Ensure that your Houdini has the Houdini Engine Plugin installed

3. Open Houdini (NOT Apprentice License), and click `Windows > Houdini Engine Session Sync > Start`

4. In Unreal Engine, click `Houdini Engine > Connect Session`. If this gives a failure message, try `Houdini Engine > Restart Session`. With the session connected, you will be able to edit values in the HDA interface and watch as Houdini processes the changes and generates an output.

5. Click the HDA actor in the level (or drag the HDA from the content browser into the level), and then in its details, click the `HoudiniAssetComponent`. This is where the HDA interface lives.

6. Change any of the values under `Houdini Parameters` or `Houdini Inputs` to kick off changes.

  **IMPORTANT: DO NOT SAVE THIS SCENE AFTER EDITING THE HDA SETTINGS** There is an ongoing issue whose cause I have not been able to identify, which causes cooked temp meshes from the HDA plugin to be generated in the wrong place/not at all. Saving or trying to bake will result in an engine freeze, and if the scene is saved at such a time, it will most probably become unusable. If this happens, in order to test the HDA you will need to make a new level, and drop the imported HDA (located in the folder `final-project\Unreal Project\BuildingGenDemo\Content\DemoFiles\Houdini`) into that scene. Despite this issue, normal HDA behavior works as expected, so you can interact with the actor in the scene, edit values in its interface, and watch as Houdini processes and updates the actor in the level. It is just saving and baking that are not usable.

# FORMATTING A CLASSIC WOW AVATAR FOR VRCHAT
--------- struggled through by daftpatience  --------  
------------------ for the blackwing guard! ---------  


things to get:

- [warcraft model viewer](https://wowmodelviewer.net/download/)

- [Blender 5.1 or 5.2](https://download.blender.org/release/)
- [CATS blender plugin 5.2.1.0](https://git.neoneko.org/NeoNeko/Cats-Blender-Plugin#install)

- [VRChat Creator Companion](https://creators.vrchat.com/sdk/)
- Unity (follow steps during VCC install for this)

-------------------------
# 🧙‍♂️ WMV PHASE 💾

- make your character in WMV! i didnt do this myself so i cant say anything helpful. [murog linked this tut](https://blenderartists.org/t/quick-tutorial-how-to-export-world-of-warcraft-assets-to-render-with-cycles/531730)

- export your avatar as an obj file.
- if you want to have multiple toggleable outfits, then first export only your avatar in underwear or no clothes at all as the base body (otherwise you'll be missing parts, like legs under skirts. that's fine if you don't want them to clip but it's more work to put them back later.)
- you can do multiple hairstyles but they're tied to the outfit that goes with them.
- export every variant you want. sort out and rename the textures for each part so that organizing those materials in unity isnt a nightmare later.

specific fun one that i recommend: make a variant of your avatar that is dressed kinda ghostly. (later i will go over making a dead avatar but we need a custom outfit slot for it. i couldn't figure out how to overlay one shader onto another and this is 2nd best.)

all base body textures should be named something like:

	base_body.tga
	base_hair.tga
	etc.

and all clothing textures should be named grouped something like:

	party_body.tga
	party_shoulder.tga
	party_helm.tga
	party_cape.tga

	dead_body.tga
	dead_shoulder.tga
	dead_helm.tga

	fishing_body.tga
	fishing_shoulder.tga
	fishing_helm.tga

etc. etc. etc. whatever you call your outfits as long as theyre nicely grouped youre good.

-------------------------
# 🧡 BLENDER PHASE 😇

don't forget to get [CATS blender plugin!](https://git.neoneko.org/NeoNeko/Cats-Blender-Plugin#install)  
it will help immensely with the armature and weight painting later.

## AVATAR PREP

- in a new blender file, drag in the body obj &  hit import. some textures may be missing, this is ok
- name the object in the collection to something suitable (BASE or BODY or something. then when you do outfits later name those after the outfit etc etc. if you dont name these clearly now you will hate doing unity stuff later)
- go to **edit mode** (select the object and hit `tab`)
- hit `A` (select all) `> merge by distance` (this cleans up a lot of messy verts)
- toggle `face orientation` in the overlays tab (upper right dropdown menu with two eclipsing circles). the OUTSIDE of the model should be blue. if any are not, go to `mesh > normals > recalculate outside`, then select any stragglers in face select mode and do `mesh > normals > flip`.
- go to `material properties > surface > base colour` and set it to image texture by clicking the little dot (and select the right .tga) rename these materials for clarity (renaming everything might feel like overkill but UNITY HATES YOU AND YOUR FAMILY so making anything easier now is gonna save you later)
- `object data properties > geometry data > clear custom normals data` (this is VITAL for aesthetic reasons)


**for any textures with transparency (usually hair):**
- switch to **shading** tab
- find hair texture node
- attach alpha from that to the principled BSDF(or whatever shader you are using) node's alpha
- if you only want one hairstyle and exported that on the base, skip ahead to [separating the hair from the head](#separating-the-hair-from-the-head) before coming back to do the armature.



## BASE BODY ARMATURE & WEIGHTS

any VRChat avatar tutorial about adding armatures should work here.
(there's a good chance you won't need eye bones since warcraft avis just use textures)
tutorials should be VRChat avatar specific, as unity/vrchat requires certain bones with certain names. you can also try using auto rigging tools like rigify (riggify?) but make sure to compare to [VRC Rig Requirements](https://creators.vrchat.com/avatars/rig-requirements/)

the tutorial i've used is rainhet chaneru's:
- [armature](https://www.youtube.com/watch?v=2QkgqsRyRMo)
- [weight painting](https://www.youtube.com/watch?v=_PB-H7t0kHI)  
she has a whole guide series about making avatars from scratch that is one of the more thorough ones out there.

IF you want any physbones (wavy hair, clothing bits, boob jiggle, etc.) you can add bones at this stage.  
do the same parent > keep offset thing and attach them to appropriate anchors. cape > chest, skirt > hips, hair > head, etc. if you have multiple outfits that can fit the same bones, make sure to weight paint them properly.



## PREPPING OUTFITS AND HAIR

- **go back to object mode.** hide your base avatar for now & repeat all AVATAR PREP steps for dressed avatars you exported earlier. it may be easier to do one of these at a time, so proceed to:


### separating the hair from the head:

- in **edit mode** go to `material properties`, click on hair texture & click the select button
  (this will select all faces that use the texture)
- if you only want one hairstyle on the base body: hit `Y` (split) to split the hair from the head without separating it.
  (this will be useful for blendshapes to hide it under hats etc.)
- if you have multiple hairstyles *don't* split it. just select the material and proceed:



### isolating clothing/accessories:

-select all accessory textures using the same method as selecting the hair
  (shoulderpads, capes, helms, etc.)
- carefully select all faces that aren't the same as the base body. everything but the base body should be selected
  (tabbards, skirts, wide sleeves, etc. you can toggle visibility of the base body to compare)
- hit `ctrl+I` to invert selection
- hit `del > faces` to delete all the redundant body parts. some of the outfit will go away, this is okay! we are going to make a toggle later to change the texture of the base body to match each outfit. that way there's no clipping outfit layers against *most* of the body, at least.
- for now if you want to view the base body with the right textures in blender, select the base body & go to the material properties tab:
  - hit the `+` to add a new material
  - hit the browse button (little checkered ball with a dropdown) & link the existing outfit material
  - click on the base body material & hit the `select` button
  - click on the outfit material & hit assign
(i'm often flipping between outfits and doing this to check stuff as i go)



### parenting outfit objects to the base armature

- `modifier properties > add modifier > deform > armature`
- select your armature in the modifier's object window there
- in scene collection, drag outfit object over the armature, hold shift & drop to parent it to the armature. now the outfit is ready to weight paint.



## VISEMES, SHAPEKEYS/BLENDSHAPES

visemes are the mouth shapes that VRChat uses to make your avi talk. these are made with blendshapes/shapekeys, and they deform the mesh in little saved poses without ruining the base model. you can also use shapekeys to make other parts of the mesh move, which you can later turn into a toggle or a part of a toggle in unity. things like smushing the hair inside a helmet, or facial expressions.

technically visemes aren't necessary. if you make a single jaw bone parented to you head armature and weight paint it only to the jaw area (so that it doesn't snag the upper lip) VRChat will use that instead and you'll have a basic jaw flap animation for talking. DON'T do this if you make viseme blendshapes for the mouthsounds or they'll stack and you'll look horrific.

- [viseme/shapekey tutorial](https://www.youtube.com/watch?v=8hCXeUSisFU&t=1489s)
(she doesn't go over every viseme, but the setup info is good. HIGHLY recommend just grabbing a mirror and referencing your own face!)(also, it starts with a blinking pose, but WoW characters just have a funny little eyelid texture that blips in, so feel free to ignore/delete that if you don't wanna bother with it. if you do wanna bother, maybe try using the knife tool to cut out the shape of the eyelid better and then bury it above the eye to make a blendshape where you pull it down like blinds haha.)
- [list of VRChat required visemes](https://wiki.vrchat.com/wiki/Visemes) note that unity prefers `i`, `o`, and `u`, to be named `ih`, `oh`, and `ou`, or you'll have to manually assign those later.




## EXPORTING AVATAR

MAKE SURE youre COMPLETELY HAPPY with all this before moving on! Unity is not as friendly as blender about making alterations.

- ensure all outfit layers are visible.
- ensure body texture is assigned to your nude/underwear base.
- `file > export > FBX`
-  under Transform, set `Up` to `Y Up`, and `Forward` to `-Z Forward`
- ENSURE the applied scaling is `FBX ALL` and not whatever it's defaulted to. if you dont do this unity will pretend everything is cool right until youre almost done and then it will throw your whole project in the garbage

name that fbx file something very clear so it's easy to find.



-------------------------
# 🔪 UNITY PHASE 👿

welcome to hell... i'm much less confident in unity. i'll list everything i did... hopefully it all works.
good luck 😭

the [rainhet tutorial](www.youtube.com/watch?v=7MDyVTpxW8) has much of this if you need visuals. it's not as specific to our purposes and it's an outdated VCC install process so be sure to follow along with what's written here!


## VCC SETUP

once you have the VRChat Creator Companion, follow all the settings in there and it should walk you through getting a compatible version of unity installed as well. make sure you have all these packages/prefabs installed with VCC (just find `Add to VCC` button and it'll do the rest):

- [vrcfury](https://vrcfury.com/download) (for making toggles)
- [poiyomi toon](https://www.poiyomi.com/) (for shaders)
- Gesture Manager (already should be in VCC)(for testing menus)
- [Easy Expressions Manager](https://embercat.gumroad.com/l/VRCHATEASYEX) (optional but what a lifesaver if you're adding more prefabs like gogoloco and syncdances and stuff. lets you reorganize your radial menu easily)

everytime you start an avatar project it'll show you packages you can toggle + on. make sure these ones are added!

most of these, once installed and added to a project, will live under the `Tools` menu in unity. you can even add things to the project while it's open if you go back to the VCC window.


## AVATAR IMPORT/SETUP

- start a 2022 avatar project and add all the prefabs you need.
- drag your FBX into the **project** panel assets folder.
- drag the imported FBX into the ** **hierarchy**** panel.
- back in the **project** panel's assets folder, select the FBX.
- now we look at the **inspector** panel.
- in the *model* tab, check `Legacy Blend Shape Normals` on and apply.
- in the *rig* tab, change animation type to `Humanoid` and apply.
- in the *materials* tab, change location to Use `External Materials` and apply. this will make a materials folder in your assets.
- in the materials folder, `right click > create > folder` name it Textures

- drag ALL your textures into that folder. we will apply them where they belong later.


## CONFIGURING RIG

- select avatar FBX in **project** panel assets folder and look at the **inspector**.
- in the *rig* tab, hit `Configure...` (it may prompt you to save.)
- inspect all the bones (there are little buttons to see the head and hands and stuff) for any mismatched connections. If you named everything carefully, it should all be good but errors can happen. eg. i had to assign the chest. make sure to hit apply and then done to get out of there.
- it's okay if your avatar doesn't have the same amt or kinds of bones as unity shows available to assign. many wow models do not have all 5 fingers, and you might have more due to physbones - they don't need to be assigned. [check VRC creator companion for the absolutely necessary ones](https://creators.vrchat.com/avatars/rig-requirements/)


## PREPPING MATERIALS

(i'm using poiyomi toon shaders for this - other shaders will work but the settings are different and idk them.)
make sure you are logged in to the VRChat SDK with the builder window visible - there's usually a warning about the textures that has an auto fix button. as soon as it shows up make sure to hit it or it's gonna make you redo all this hard work later.

- in the **project** view's materials folder there should be many gray orbs with familiar names. pick one and look at the **inspector**.
  - `unlock shader` if it is locked.
  - change shader from standard to `.poiyomi > poiyomi toon` open the dropdown.
    - `Color & Normals > Texture` has a blank box. from the **project** view's textures folder, drag the correct texture on to that.
    - uncheck `Rendering > Culling and Depth > Cull` so backfaces are visible.
    - at the bottom of the shader options, find `VRChat Fallback Shader` change it to `standard > opaque > doublesided`
    - if the texture requires transparency, find the `Alpha Options` dropdown menu in `Color & Normals` and uncheck `force opaque`.
  - `lock in optimized shader`

repeat these steps for every material except your dead/ghost one, if you made one of those.

for now the main body won't be connected to all the outfit materials, we will add toggles for that soon.

### GHOSTLY material for when you're dead

  I made a slightly more complicated shader for the dead outfit. it's really simple and doesn't do a lot of what i want so i encourage everyone to mess around... maybe we can figure out some effects... we're restricted by the need for backfaces to be visible with these avatars.

  here's a really quick rundown of are all the toggles i did. since vi only had one hairstyle i also had to make a new material for the dead version of the hair.

all ghost mats:
- poiyomi toon
- top right setting change from opaque to transparent
- `Color & normals > Color and alpha` (RGBA 105 112 128 150)
- `alpha options > uncheck force opaque`
- `Special FX > emissions >` check & open `emission 1` (0 doesnt do anything)
	- if material has transparency like hair, add emission mask texture
		(make a copy of the hair & turn hair white and background black in whatever art program)
	- `Emission Color` (RGBA 134 178 183 255)
	- `Emission strength` 5
	- check `use base colours` ON
- `rendering > cull` off, `ZClip` on
- `fallback: standard > transparent > doublesided`

right click on the shader and copy material properties. paste this onto all your dead materials (you'll have to replace the textures each time!
it took a little fiddling to get the hair to behave. I found copying the material properties of the normal hair and pasting it on the ghost hair and then changing it back to transparent worked.)


## AVATAR DESCRIPTOR

this is the absolute base important thing you need for your avatar to work in VRChat.

- select your FBX in the  **hierarchy** and look at **inspector**.
- click `Add Component` and search for `VRC Avatar Descriptor`.
  - `view > edit view position` you'll see a little floating sphere with a gimbal to move around in the scene. align this vertically with the eyeline of your avatar, and align the depth with approximately the eyesockets. keep it centered.
  - `Lipsync > Auto Detect` check that all your visemes got copied correctly and assign any missing ones.
  - if you kept the eyelid texture, open the `eye look > eyelids` dropdown:  
		- `eyelid type`: drag in the body mesh from the hierarchy  
		- under `blendshape states` set `blink` to body mesh and `looking up`/`looking down` to `none`

TECHNICALLY you can call this done & upload it! it's playable now but it'll be wearing every outfit at once. let's keep adding stuff >:D



## OUTFIT SWAP TOGGLES

- select avatar in  **hierarchy** (you should see a blue selection outline) and look at the **inspector**.
- hit `Add Component` and search `Toggle (VRCFury)`
- in *menu path* field, type `outfits/fishingoutfit` or whatever you want to call it. the `outfits/` part makes a subfolder on your radial menu, and you can tell other items to follow this same file path.
- `Add Action > Object Toggle` (we are showing/hiding outfit parts)
  -`Turn On:` drag in the outfit from the  **hierarchy** (this is your shoulderpads/helmet/skirt/tabbard/cape/whatever. now that it is set to ON that means its OFF everywhere else but here.)
- Add Action `+` (this is now a tiny plus rather than a big button) `> Material Swap` (we are changing the texture on the body)
  - set `Renderer` to your base body object. (you can drag it in from the  **hierarchy**)
  - set `slot` to the base body texture slot (nude/underwear/whatever you called it)
  - set `material` to the outfit material you want to change it to. (you can drag it in from the materials folder in the **project** view)
- IF you made any blendshapes to go with this outfit (like hat hair) Add action `+ > Blendshape` and select the correct blendshape.
- `options > saved between worlds`
- `options > Enable Exclusive Tags` (this will make it so toggling an outfit on automatically turns the others off)
  - in *Exclusive tag* field, type "outfit" or something


now we repeat all that for the remaining outfits:
- `right click` on your prev toggle, hit `Copy Component`.
- `right click` in the same space `> Paste Component As New`
- make a *menu path* name (outfits/partyoutfit)
- change `object toggle` to new outfit
- change material swap's `material` to new outfit material
- add or remove any blendshapes that need adding or removing
(leave `renderer` and `slot` as is)

if you want your underwear as an outfit:
- paste another of the same VRCFury toggle
- make a *menu path* name (outfits/undies)
- delete any actions. the rest of the outfits turn on the clothing so now they are automatically OFF for this one, and the material is already correct.
- `options > This Is Exclusive Off State`

if you don't want the underwear as an outfit, choose one of the other outfit toggles and set that one to `Default On` AND `This Is Exclusive Off State`

you can test all these toggles by adding `tools > gesture manager`. It's a good idea to check your work as you go.


## OPTIONAL PREFABS

adding premade prefabs like this isn't too hard, they usually come with tutorials. most of the time you just find the right prefab and drag it on to the character in the hierarchy. you can do it i believe in you

- [gogoloco](https://gogoloco.net/) lets you do a lot of nice sitting/lying poses in and out of VR and also adjust your position in space (flying!)
(it's honestly a little easier to download it from gumroad than to install it with VCC but you can do it that way too.)
- [syncdance](https://kiny.booth.pm/items/4881102)

check out places like Booth, Jinxxy, Nexxy, gumroad etc. if you want to find more things to put on your avi.


## OPTIONAL PHYSBONES

[this tutorial](https://www.youtube.com/watch?v=PTTnWUkswkU) goes over it all except a couple of new things you can add (squash and stretch). if you made any physbones earlier in blender and have been wondering when they come into play, now is their time to shine.

if the scalings of the `collision radius` seem really, really weird (like they're gigantic unless you bring them down to .00001 size) then you didn't export the fbx with the right scaling settings (fbx all) and you're gonna have to start over in unity from the top or give up on physbones. don't ask me why i know this.

`gesture manager` is exceptionally useful for testing these!


## UPLOADING AVATAR

open `VRChat SDK > Control Panel` if it isn't still open. you should have already logged in earlier. go to the *builder* tab and make sure the right avatar is selected. name it! write a desc! capture a pic from the scene!

as long as the warning panel is only yellows, no red, then you're good to upload your avatar!


thanks for reading all this!! i hope it isn't too difficult. feel free to message me if you need help at any point in the process! i'd be so happy to help prep anything! especially the blender stuff. armatures are fun to me :D







### SOMEDAY WE'LL FIGURE OUT

- held/droppable items
- cool effects like smoke and flames and stuff
- maybe somehow exporting and applying animations... that's some big brained stuff...

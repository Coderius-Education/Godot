---
sidebar_position: 11
---

# Animations in GDScript

## Informatie opvragen
We hebben drie stukjes informatie nodig:
1. Bevindt onze hoofdpersoon zich op de grond?
2. Naar welke kant beweegt onze hoofdpersoon zich?
3. Staat onze hoofdpersoon stil?

We gaan uiteraard drie `variabelen` aanmaken om deze informatie bij te houden.
Plaats dit bovenaan je script (onder `JUMP_VELOCITY` en boven `_physics_process`):

```gdscript
var op_de_grond
var kijkt_naar_rechts
var staat_stil
```

<details>
    <summary>Kijk hier voor de oplossing!</summary>

```gdscript
extends CharacterBody2D

const SPEED = 300.0
const JUMP_VELOCITY = -800.0

var op_de_grond
var kijkt_naar_rechts
var staat_stil

func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var direction := Input.get_axis("ui_left", "ui_right")
	if direction:
		velocity.x = direction * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)		
		
	move_and_slide()
```
</details>

## op_de_grond
Welke functie kunnen we gebruiken om te weten of onze hoofdpersoon op de grond staat?
- Geef de variabele `op_de_grond` de waarde van deze functie.
- Hoe zou je in GDScript kunnen zeggen dat als je hoofdpersoon niet op de grond staat, dat dan de animatie `jump` afgespeeld moet worden?

<details>
    <summary>Kijk hier voor de oplossing!</summary>

```gdscript
extends CharacterBody2D

const SPEED = 300.0
const JUMP_VELOCITY = -800.0

var op_de_grond
var kijkt_naar_rechts
var staat_stil

func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var direction := Input.get_axis("ui_left", "ui_right")
	if direction:
		velocity.x = direction * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)	
	
	op_de_grond = is_on_floor()
	if not op_de_grond:
		$Sprite2D.play('jump')
		
	move_and_slide()
```

</details>

## Stilstaan
`velocity.x` houdt de beweging op de horizontale as bij.
- Bij welke waarde van `velocity.x` staat je hoofdpersoon stil?
- Hoe zou je de animatie `idle` aan kunnen zetten als de hoofdpersoon stilstaat?

<details>
    <summary>Kijk hier voor de oplossing!</summary>

```gdscript
extends CharacterBody2D

const SPEED = 300.0
const JUMP_VELOCITY = -800.0

var op_de_grond
var kijkt_naar_rechts
var staat_stil

func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var direction := Input.get_axis("ui_left", "ui_right")
	if direction:
		velocity.x = direction * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)	
	
	staat_stil = velocity.x == 0	
	if staat_stil:
		$Sprite2D.play('idle')
	op_de_grond = is_on_floor()
	if not op_de_grond:
		$Sprite2D.play('jump')
		
	move_and_slide()
```

</details>

## Richting
`velocity.x` houdt de beweging op de horizontale as bij.
- Bij welke waarde van `velocity.x` beweegt je hoofdpersoon naar links? En bij welke waarde naar rechts?
- Hoe zou je de animatie `run` aan als je hoofdpersoon niet stilstaat?
- We gebruiken `$Sprite2D.flip_h = true` om je afbeelding te spiegelen (dus dat de hoofdpersoon naar links kijkt in plaats van naar rechts). Hoe zou je dit kunnen gebruiken in je script?

<details>
    <summary>Kijk hier voor de oplossing!</summary>

```gdscript
extends CharacterBody2D

const SPEED = 300.0
const JUMP_VELOCITY = -800.0

var op_de_grond
var kijkt_naar_rechts
var staat_stil

func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction and handle the movement/deceleration.
	# As good practice, you should replace UI actions with custom gameplay actions.
	var direction := Input.get_axis("ui_left", "ui_right")
	if direction:
		velocity.x = direction * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)	
	
	staat_stil = velocity.x == 0	
	if staat_stil:
		$Sprite2D.play('idle')
	op_de_grond = is_on_floor()
	if not op_de_grond:
		$Sprite2D.play('jump')
		
	if velocity.x > 0:
		$Sprite2D.play('run')
		$Sprite2D.flip_h = false	
	if velocity.x < 0:
		$Sprite2D.play('run')
		$Sprite2D.flip_h = true	
		
	move_and_slide()
```

</details>
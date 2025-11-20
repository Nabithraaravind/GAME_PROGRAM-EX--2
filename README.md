# GAME_PROGRAM-EX--2
## CREATE A PLAYER MOVEMENT USING CHARACTER, COLLECTABLE, PLAYER HEALTH AND SCORE

## NAME : A.NABITHRA
## REGISTER NO.: 212224230172

## Aim
Create a playable third-person character in Unreal Engine that can move and run, collect coin-like collectibles, track a Score and Player Health, and display both on-screen (UI).
Overview

1.Player Character (BP_PlayerCharacter) — handles movement, input, health, and overlaps with collectables.
2.Collectable (BP_Collectable) — simple actor with collision that gives score (and optionally health) when overlapped.
3.UI (WBP_HUD) — UMG Widget showing Score and Health values.
4.GameMode / PlayerState — (optional) hold persistent Score/HighScore across respawns.
5.Game flow — pickup increments Score, maybe plays sound/particle and destroys the collectable; health decreases on damage, and player dies or respawns when health ≤ 0.

Step-by-step Implementation (Blueprint-first)
Project & Input Setup

•Create a Third Person Blueprint project (or use your existing ThirdPersonMap).
•Open Project Settings → Input and ensure these mappings exist:

○ MoveForward (W / Up arrow)
○ MoveRight (A/D or Left/Right)
○ Turn / LookUp (mouse)
○ Jump (SpaceBar)
○ Run (Left Shift) — optional if you want sprint

Player Character Blueprint (BP_PlayerCharacter)

•Duplicate the existing ThirdPersonCharacter (or create a new Character blueprint) and name it BP_PlayerCharacter.
•Variables to add (Expose where useful):

○ Score (Integer) — default 0
○ MaxHealth (Float) — e.g. 100.0
○ Health (Float) — default equal to MaxHealth
○ bIsRunning (Boolean) — if you want sprinting

•Movement (in Event Graph):

○ Use Add Movement Input hooked to MoveForward and MoveRight axis mappings.
○ Use Turn and LookUp to rotate camera.
○ If using Run: on Run Pressed set Max Walk Speed on the Character Movement component (e.g. 1200) and reset on Released (600 default).

•Health functions:

→ Function: ApplyDamage(float DamageAmount)

○ Subtract DamageAmount from Health.
○ If Health <= 0 → call OnDeath event (disable input, play animation, respawn or show Game Over).
○ Update HUD (call event to update widget binding).

→ Function: AddHealth(float HealAmount)

○ Add to Health but clamp to MaxHealth.
○ Update HUD.

•Score management:

○ Function: AddScore(int Amount)
○ Score = Score + Amount → update HUD.

•BP_Collectable → OnComponentBeginOverlap (Sphere)

○ Other Actor → Cast To BP_PlayerCharacter
○ Branch (if cast success)
○ Call AddScore(ScoreValue) on Player Character
○ If GiveHealth > 0 Call AddHealth(GiveHealth)
○ Play Sound at Location
○ Spawn Emitter at Location
○ Destroy Actor

•BP_PlayerCharacter → AddScore (Custom Event)

○ Input: Amount (int)
○ Score = Score + Amount
○ Call UpdateScoreDisplay on the HUD widget reference
○ (Optional) Play pickup sound, animate, or show floating text

•BP_PlayerCharacter → ApplyDamage (Custom Event)

○ Input: Damage (float)
○ Health = Health - Damage
○ If Health <= 0
○ Call OnDeath (Disable Input; show Game Over)
○ Update HUD: Call UpdateHealthDisplay

## Output
<img width="572" height="224" alt="Screenshot 2025-11-19 140717" src="https://github.com/user-attachments/assets/ec350179-8f39-4318-8640-d6341770e942" />
<img width="777" height="487" alt="Screenshot 2025-11-19 140735" src="https://github.com/user-attachments/assets/6d806e49-5844-4160-8c8a-4f2a59956fc5" />
<img width="778" height="467" alt="Screenshot 2025-11-19 140749" src="https://github.com/user-attachments/assets/0b9c920e-3514-4e9c-af9d-a5c5193beb8d" />
<img width="776" height="376" alt="Screenshot 2025-11-19 140759" src="https://github.com/user-attachments/assets/f5433873-ce37-4f7c-ab58-cbe4af1d3529" />

## Result
The AI character successfully roams within the defined NavMesh area, choosing random destinations at intervals using the Behavior Tree logic.


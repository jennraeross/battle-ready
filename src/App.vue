<script setup>
import Card from './components/Card.vue'
import MenuItem from './components/MenuItem.vue'
import Button from './components/Button.vue'
//import Table from './components/Table.vue'
//import TableRow from './components/TableRow.vue'
//import TableCell from './components/TableCell.vue'
//import TextBox from './components/TextBox.vue'
//import Select from './components/Select.vue'
</script>

<template lang="pug">
.grid(class="sm:grid-cols-1 md:grid-cols-4")
    aside.sidebar.flex.flex-col.border-r(class='md:shadow-md md:h-screen md:overflow-y-auto')
        .my-4.border-b-4.border-gray-400.text-center.m-4
            span.font-mono.text-xl.font-bold.tracking-widest
                span.text-gray-600 Battle Ready
        MenuItem Character 1
        MenuItem Character 2
        MenuItem {{ name }}
        Button.mx-auto.my-4 Create Character


    main.main.flex.flex-grow.flex-col.p-4.items-center(class="md:col-span-3 md:h-screen md:overflow-y-auto")
        Card(class="!p-0 sm:grid-cols-1 md:grid-cols-2").w-full.grid
            .flex.flex-col
                .p-4.bg-gray-50.border-b.rounded-tl-md
                    p Name 
                .p-4.border-b(class="md:border-b-0")
                    input.border.rounded.w-full.p-2(type="text" v-model="name" placeholder="Jane Doe")
            .flex.flex-col
                .p-4.bg-gray-50.border-b.rounded-tr-md
                    p Level 
                .p-4 
                    select.w-full.border.rounded.p-2(v-model="level") 
                        option(v-for="n in 20" :value='n') {{n}}

        Card(class="!p-0 sm:grid-cols-1 md:grid-cols-3").w-full.grid.mt-4
            .flex.flex-col
                .p-4.bg-gray-50.border-b.rounded-tl-md
                    p Background 
                .p-4.border-b(class="md:border-b-0")
                    select.w-full.border.rounded.p-2(v-model="backgroundSelected") 
                        option(v-for="background in backgrounds" :value='background') {{background}}
            .flex.flex-col
                .p-4.bg-gray-50.border-b
                    p Ancestry 
                .p-4.border-b(class="md:border-b-0")
                    select.w-full.border.rounded.p-2(v-model="raceSelected") 
                        option(v-for="race in races" :value="race.name") {{race.name}}
            .flex.flex-col
                .p-4.bg-gray-50.border-b.rounded-tr-md
                    p Lineage 
                .p-4 
                    select.w-full.border.rounded.p-2(v-model="subraceSelected" v-if="raceSelected")
                        option(v-for="subrace in getSubraces()" :value="subrace.name") {{subrace.name}}
                    select.w-full.border.rounded.p-2(v-if="!raceSelected" disabled)

        
        Card(class="!p-0 sm:grid-cols-1 md:grid-cols-2").mt-4.w-full.grid
            .flex.flex-col
                .p-4.bg-gray-50.border-b.rounded-t-md(class="md:rounded-tr-none")
                    p Class 
                .p-4.border-b(class="md:border-b-0")
                    select.w-full.border.rounded.p-2(v-model='className')
                        option {{className}}
            .flex.flex-col
                .p-4.bg-gray-50.border-b(class="md:rounded-tr-md")
                    p Subclass 
                .p-4.border-b(class="md:border-b-0")
                    select.w-full.border.rounded.p-2(v-model='subclassSelected')
                        option(v-for='subclass in subclasses' :value='subclass.name') {{subclass.name}}


        Card(class="!p-0 sm:grid-cols-2 md:grid-cols-3").mt-4.w-full.grid
            .flex.flex-col
                .p-4.bg-gray-50.border-b.rounded-tl-md.rounded-tr-md.text-center(class="sm:rounded-tr-none")
                    p Strength 
                .p-4.border-b
                    input.w-full.border.rounded.p-2.text-center(type="text" v-model="str")
            .flex.flex-col
                .p-4.bg-gray-50.border-b.text-center(class="sm:rounded-tr-md md:rounded-tr-none")
                    p Dexterity
                .p-4.border-b
                    input.w-full.border.rounded.p-2.text-center(type="text" v-model="dex")
            .flex.flex-col
                .p-4.bg-gray-50.border-b.text-center(class="md:rounded-tr-md")
                    p Constitution
                .p-4.border-b
                    input.w-full.border.rounded.p-2.text-center(type="text" v-model="con")
            .flex.flex-col
                .p-4.bg-gray-50.border-b.text-center
                    p Intelligence
                .p-4.border-b(class="md:border-b-0")
                    input.w-full.border.rounded.p-2.text-center(type="text" v-model="int")
            .flex.flex-col
                .p-4.bg-gray-50.border-b.text-center
                    p Wisdom
                .p-4.border-b(class="md:border-b-0")
                    input.w-full.border.rounded.p-2.text-center(type="text" v-model="wis")
            .flex.flex-col
                .p-4.bg-gray-50.border-b.text-center
                    p Charisma 
                .p-4
                    input.w-full.border.rounded.p-2.text-center(type="text" v-model="cha")

        Card(class="!p-0 md:grid-cols-12").w-full.mt-4.grid.grid-cols-6
            .p-4.border-b.bg-gray-50.rounded-t.col-span-6(class="md:col-span-12")
                p Skill Proficiencies
            .p-4.border-b.text-center
                p P 
            .p-4.border-b.text-center
                p E 
            .p-4.border-b.col-span-4 
                p Skill Name
            .p-4.border-b.text-center.hidden(class="md:block")
                p P 
            .p-4.border-b.text-center.hidden(class="md:block")
                p E 
            .p-4.border-b.col-span-4.hidden(class="md:block")
                p Skill Name
            template(v-for='skill in proficiencies')
                .px-4.py-2.text-center
                    input(type='checkbox' v-model='skill.proficient')
                .px-4.py-2.text-center
                    input(type='checkbox' v-model='skill.expert' v-if="skill.proficient")
                .px-4.py-2.col-span-4
                    span &nbsp;{{skill.name}}&nbsp;

        Card(class="!p-0").w-full.mt-4
            .p-4.border-b.rounded-t-md.bg-gray-50
                p Armor 
            .p-4
                select.w-full.border.rounded.p-2(v-model='armorSelected')
                    template(v-for='armor in armorTypes.unarmored')
                        option(:value='armor') {{armor.name}}
                    template(v-for='armor in armorTypes.light')
                        option(:value='armor') {{armor.name}}
                    template(v-for='armor in armorTypes.medium')
                        option(:value='armor') {{armor.name}}
                    template(v-if="subclassSelected == 'Commander' || subclassSelected == 'Paragon'" v-for='armor in armorTypes.heavy')
                        option(:value='armor') {{armor.name}}
            .p-4.border-t(v-if="subclassSelected == 'Commander' || subclassSelected == 'Paragon' || subclassSelected == 'Chieftain'")
                input(type="checkbox" v-model="shieldSelected")
                span &nbsp;Shield

        Card.w-full.mt-4.grid.grid-cols-3(class="!p-0 md:grid-cols-6")
            .flex.flex-col.col-span-3
                .p-4.border-b.rounded-t-md.bg-gray-50(class="md:rounded-tr-none")
                    p Weapon Name 
                .p-4.border-b
                    input.w-full.border.rounded.p-2(type="text" v-model="weaponName" placeholder="Weapon Name")
            .flex.flex-col
                .p-4.border-b.bg-gray-50.text-center
                    p Damage 
                .p-4.text-center.h-full.border-b
                    select.w-full.border.rounded.p-2(v-model='weaponDie')
                        option 1d4
                        option 1d6
                        option 1d8
                        option 1d10
                        option 1d12
                        option 2d4
                        option 2d6 
            .flex.flex-col
                .p-4.border-b.bg-gray-50.text-center
                    p Finesse 
                .p-4.flex.flex-col.align-center.justify-center.h-full.border-b
                    input(type='checkbox' v-model='weaponIsFinesse')
            .flex.flex-col
                .p-4.border-b.bg-gray-50.text-center(class="md:rounded-tr")
                    p Ranged 
                .p-4.flex.flex-col.align-center.justify-center.h-full.border-b
                    input(type='checkbox' v-model='weaponIsRanged')
            .p-4.col-span-3(class="md:col-span-6")
                input(type="checkbox" v-model="offhandWeapon")
                span &nbsp;Dual Wielding
            .flex.flex-col.col-span-3.border-t(v-if="offhandWeapon")
                .p-4.border-b.rounded-t-md.bg-gray-50(class="md:rounded-tr-none")
                    p Offhand Weapon 
                .p-4.border-b
                    input.w-full.border.rounded.p-2(type="text" v-model="offhandWeaponName" placeholder="Weapon Name")
            .flex.flex-col.border-t(v-if="offhandWeapon")
                .p-4.border-b.bg-gray-50.text-center
                    p Damage 
                .p-4.text-center.h-full.border-b
                    select.w-full.border.rounded.p-2(v-model='offhandWeaponDie')
                        option 1d4
                        option 1d6
                        option 1d8
                        option 1d10
                        option 1d12
                        option 2d4
                        option 2d6 
            .flex.flex-col.border-t(v-if="offhandWeapon")
                .p-4.border-b.bg-gray-50.text-center
                    p Finesse 
                .p-4.flex.flex-col.align-center.justify-center.h-full.border-b
                    input(type='checkbox' v-model='offhandWeaponIsFinesse')
            .flex.flex-col.border-t(v-if="offhandWeapon")
                .p-4.border-b.bg-gray-50.text-center(class="md:rounded-tr")
                    p Ranged 
                .p-4.flex.flex-col.align-center.justify-center.h-full.border-b
                    input(type='checkbox' v-model='offhandWeaponIsRanged')

        Card(class="!p-0" v-if="subclassSelected == 'Paragon' || (subclassSelected == 'Packleader' && level >=3)").w-full.mt-4
            .p-4.rounded-t-md.bg-gray-50
                p Optional Features
            div(v-for='feature in subclasses.paragon.features' v-if="subclassSelected == 'Paragon'")
                .p-4(v-if='feature.optional').border-t
                    input(type='checkbox' v-model='feature.chosen')
                    span &nbsp;{{feature.name}}
            div(v-if="subclassSelected == 'Packleader' && level >= 3")
                .p-4.bg-gray-50 
                    p Spell Selection:
                .p-4.border-t 
                    label Extra Cantrip:
                    input.w-full.border.rounded.p-2(type="text" v-model="packleaderCantrip" placeholder="Cantrip Name")
                .p-4.border-t 
                    label Extra Spell:
                    input.w-full.border.rounded.p-2(type="text" v-model="packleaderSpell" placeholder="Spell Name")
            div(v-if="subclassSelected == 'Packleader' && level >= 18")
                .p-4.border-t
                    label Warlord of the Wilds Spells:
                    input.w-full.border.rounded.p-2(type="text" v-model="packleaderSpell2" placeholder="Spell Name")
                    input.w-full.border.rounded.p-2.mt-2(type="text" v-model="packleaderSpell3" placeholder="Spell Name")
                    input.w-full.border.rounded.p-2.mt-2(type="text" v-model="packleaderSpell4" placeholder="Spell Name")
                    input.w-full.border.rounded.p-2.mt-2(type="text" v-model="packleaderSpell5" placeholder="Spell Name")
                    input.w-full.border.rounded.p-2.mt-2(type="text" v-model="packleaderSpell6" placeholder="Spell Name")

        br
        br
        .border.border-gray-300.w-full
        br

        Card#sheet(class="!p-0").w-full.mt-4
            .w-full.p-4.bg-gray-50.rounded-t-md.border-b.text-center
                p Character Sheet
            .w-full.grid.grid-cols-2
                .p-4
                    h4 {{name}}
                .p-4.text-right
                    h5(v-if="raceSelected") #[span(v-if="subraceSelected") {{subraceSelected}}] {{raceSelected}}
                .p-4.col-span-2.border-y
                    h5 {{className}} {{level}} #[span(v-if="subclassSelected") -] {{subclassSelected}}
            .w-full.grid.grid-cols-2(class="md:grid-cols-4")
                .flex.flex-col.text-center
                    .p-4.bg-gray-50.border-b 
                        p Initiative 
                    .p-4.border-b 
                        p {{ initiative() }}
                .flex.flex-col.text-center
                    .p-4.bg-gray-50.border-b 
                        p Armor Class 
                    .p-4.border-b 
                        p {{ AC() }}
                .flex.flex-col.text-center
                    .p-4.bg-gray-50.border-b 
                        p Hit Points 
                    .p-4.border-b 
                        p {{ hp() }}
                .flex.flex-col.text-center
                    .p-4.bg-gray-50.border-b 
                        p Proficiency 
                    .p-4.border-b 
                        p {{ proficiency() }}
            .w-full.p-4.text-center.bg-gray-50.mt-4.border-t
                p Stats
            .w-full.grid.grid-cols-3(class="md:grid-cols-6")
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p STR 
                    .p-4.border-b.font-bold
                        p #[span(v-if="mod(str) > 0") +]{{mod(str)}}
                    .p-4.border-b 
                        p {{ str }}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p DEX 
                    .p-4.border-b.font-bold
                        p #[span(v-if="mod(dex) > 0") +]{{mod(dex)}}
                    .p-4.border-b 
                        p {{ dex }}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p CON 
                    .p-4.border-b.font-bold
                        p #[span(v-if="mod(con) > 0") +]{{mod(con)}}
                    .p-4.border-b 
                        p {{ con }}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p INT 
                    .p-4.border-b.font-bold
                        p #[span(v-if="mod(int) > 0") +]{{mod(int)}}
                    .p-4.border-b 
                        p {{ int }}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p WIS 
                    .p-4.border-b.font-bold
                        p #[span(v-if="mod(wis) > 0") +]{{mod(wis)}}
                    .p-4.border-b 
                        p {{ wis }}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p CHA 
                    .p-4.border-b.font-bold
                        p #[span(v-if="mod(cha) > 0") +]{{mod(cha)}}
                    .p-4.border-b 
                        p {{ cha }}
            .w-full.p-4.text-center.bg-gray-50.mt-4.border-t
                p Saves
            .w-full.grid.grid-cols-3(class="md:grid-cols-6")
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p STR 
                    .p-4.border-b 
                        p #[span(v-if="save(str, saves.str) > 0") +]{{save(str, saves.str)}}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p DEX 
                    .p-4.border-b 
                        p #[span(v-if="save(dex, saves.dex) > 0") +]{{save(dex, saves.dex)}}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p CON 
                    .p-4.border-b 
                        p #[span(v-if="save(con, saves.con) > 0") +]{{save(con, saves.con)}}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p INT 
                    .p-4.border-b 
                        p #[span(v-if="save(int, saves.int) > 0") +]{{save(int, saves.int)}}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p WIS 
                    .p-4.border-b 
                        p #[span(v-if="save(wis, saves.wis) > 0") +]{{save(wis, saves.wis)}}
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p CHA 
                    .p-4.border-b 
                        p #[span(v-if="save(cha, saves.cha) > 0") +]{{save(cha, saves.cha)}}

            .w-full.p-4.text-center.bg-gray-50.mt-4.border-t
                p Skill Proficiencies
            .w-full.grid.grid-cols-6.border-b(class="md:grid-cols-12")
                .p-4.text-center.bg-gray-50.border-b(class="md:border-l")
                    p Prof
                .p-4.col-span-3.bg-gray-50.border-b
                    p Skill 
                .p-4.col-span-2.text-center.bg-gray-50.border-b
                    p Mod 
                .p-4.text-center.border-l.bg-gray-50.border-b.hidden(class="md:block")
                    p Prof
                .p-4.col-span-3.bg-gray-50.border-b.hidden(class="md:block")
                    p Skill 
                .p-4.col-span-2.text-center.bg-gray-50.border-b.hidden(class="md:block")
                    p Mod 
                template(v-for="skill in proficiencies")
                    .p-4.text-center(class="md:border-l")
                        p(v-if="skill.proficient") &#10003; #[span(v-if="skill.proficient && skill.expert") &#10003;]
                    .p-4.col-span-3
                        p {{ skill.name }}
                    .p-4.col-span-2.text-center
                        p #[span(v-if="skillMod(skill.stat, skill.proficient, skill.expert) >= 0") +]{{skillMod(skill.stat, skill.proficient, skill.expert)}} #[span(v-if="skill.name == 'Stealth' && armorSelected.stealthDis == true" style="color: red;") D]
            
            .w-full.p-4.text-center.bg-gray-50.mt-4.border-t 
                p Actions 
            .w-full.grid.grid-cols-4
                .flex.flex-col.col-span-2 
                    .p-4.bg-gray-50.border-b
                        p Attack
                    .p-4.border-b(v-if="weaponName && weaponDie")
                        p {{ weaponName }}
                    .p-4.border-b
                        p Unarmed Attack
                    .p-4.border-b
                        p Battlefield Presence 
                .flex.flex-col.text-center
                    .p-4.bg-gray-50.border-b
                        p Hit 
                    .p-4.border-b(v-if="weaponName && weaponDie")
                        p #[span(v-if="weaponAttackMod() >= 0") +]{{weaponAttackMod()}}
                    .p-4.border-b
                        p #[span(v-if="mod(str) + proficiency() >= 0") +]{{mod(str) + proficiency()}}
                    .p-4.border-b
                        p -
                .flex.flex-col.text-center 
                    .p-4.bg-gray-50.border-b 
                        p Damage 
                    .p-4.border-b(v-if="weaponName && weaponDie")
                        p {{weaponDie}} #[span(v-if="weaponDamageMod() > 0") &nbsp;+] #[span(v-if="weaponDamageMod() != 0") &nbsp;{{weaponDamageMod()}}]
                    .p-4.border-b
                        p #[span(v-if="mod(str) + proficiency() >= 0") +]{{mod(str) + proficiency()}}
                    .p-4.border-b
                        p -


            div(v-if="level >= 14 || (level >= 3 && subclassSelected == 'Chieftain') || (level >= 3 && subclassSelected == 'Paragon') || (level >= 3 && subclassSelected == 'Tactitian')")
                .w-full.text-center.p-4.bg-gray-50
                    p Other Actions
                .w-full.p-4.bg-gray-50.border-b
                    p Leadership Dice Ability
                .p-4.border-b(v-if="level >= 3 && subclassSelected =='Chieftain'") 
                    p Warcry
                .p-4.border-b(v-if="level >= 3 && subclassSelected =='Paragon'")
                    p Heroic Strike
                .p-4.border-b(v-if="level >= 3 && subclassSelected =='Tactitian'")
                    p Tactical Flexibility
                .p-4.border-b(v-if='level >= 14')
                    p Shift the Field
                .p-4.border-b(v-if="level >= 15 && subclassSelected =='Chieftain'")
                    p Booming Shout
                .p-4.border-b(v-if="level >= 18 && subclassSelected =='Noble'")
                    p Grand Decree
            
            div(v-if="(offhandWeapon && offhandWeaponName && offhandWeaponDie) || level >= 2") 
                .w-full.p-4.text-center.bg-gray-50.border-t.mt-4
                    p Bonus Actions
                .grid.grid-cols-2(class="md:grid-cols-4" v-if="offhandWeapon && offhandWeaponName && offhandWeaponDie")
                    .flex.flex-col.col-span-2
                        .p-4.bg-gray-50.border-b 
                            p Offhand Attack 
                        .p-4.border-b 
                            p {{ offhandWeaponName }}
                    .flex.flex-col 
                        .p-4.bg-gray-50.border-b 
                            p Hit 
                        .p-4.border-b 
                            p #[span(v-if="offhandWeaponAttackMod() >= 0") +]{{offhandWeaponAttackMod()}}
                    .flex.flex-col 
                        .p-4.bg-gray-50.border-b 
                            p Damage 
                        .p-4.border-b 
                            p {{ offhandWeaponDie }}

                div(v-if="level >= 2")
                    .p-4.bg-gray-50.border-b 
                        p Leadership Dice Abilities 
                    .p-4.border-b 
                        p Rallying Mark 
                    .p-4.border-b 
                        p Urgent Orders 
                    .p-4.border-b 
                        p Helpful Word
                    .p-4.border-b(v-if="level >= 7 && subclassSelected =='Commander'")
                        p On Your Feet!
                    .p-4.border-b(v-if="level >= 7 && subclassSelected =='Noble'")
                        p Imperative Order
                    .p-4.border-b(v-if="level >= 9")
                        p Rallying Mark: Boost Morale
                    .p-4.border-b(v-if="level >= 9")
                        p Urgent Orders: Coordinated Movements
                    .p-4.border-b(v-if="level >= 9")
                        p Helpful Word: Expert Instructions
                    .p-4.border-b(v-if="level >= 15 && subclassSelected =='Tactitian'")
                        p Urgent Orders: Advanced Orders 
                    .p-4.border-b(v-if="level >= 18 && subclassSelected =='Commander'")
                        p On Your Feet! - No One Left Behind!
                    
                div(v-if="level >= 3 && subclassSelected =='Tactitian'")
                    .p-4.bg-gray-50.border-b 
                        p Tactical Insight
                    .p-4.border-b 
                        p Determine Attack
                    .p-4.border-b 
                        p Predict Movement
                    .p-4.border-b 
                        p Outwit Response
                    .p-4.border-b  
                        p Expose Weakness

                div(v-if="level >= 3 && subclassSelected == 'Commander'") 
                    .p-4.bg-gray-50.border-b
                        p Other
                    .p-4.border-b
                        p Keen Pointer
            .w-full.p-4.bg-gray-50.border-y.mt-4.text-center
                p Reactions
            .w-full.p-4.border-b
                p Attack of Opportunity
            .w-full.p-4.border-b(v-if="level >= 7 && subclassSelected == 'Tactitian'")
                p Inscrutable Mind
            .w-full.p-4.border-b(v-if="level >= 18 && subclassSelected == 'Chieftain'")
                p Bloody Victory
            
            div(v-if="subclassSelected") 
                .w-full.p-4.bg-gray-50.border-y.mt-4.text-center
                    p Other
                .w-full.p-4.border-b(v-if="subclassSelected == 'Commander'")
                    p Form Up!
                .w-full.p-4.border-b(v-if="subclassSelected == 'Chieftain'")
                    p Wolfpack Movement
                .w-full.p-4.border-b(v-if="subclassSelected == 'Noble'")
                    p Call to Arms
                .w-full.p-4.border-b(v-if="subclassSelected == 'Packleader'")
                    p Concealed Approach
                .w-full.p-4.border-b(v-if="subclassSelected == 'Paragon'")
                    p Lead the Charge
                .w-full.p-4.border-b(v-if="subclassSelected == 'Tactitian'")
                    p Battle Plans
                .w-full.p-4.border-b(v-if="subclassSelected == 'Chieftain' && level >= 3")
                    p Savage Momentum
                .w-full.p-4.border-b(v-if="subclassSelected == 'Packleader' && level >= 7")
                    p Shroud of Nature
                .w-full.p-4.border-b(v-if="subclassSelected == 'Paragon' && level >= 7")
                    p Stand Defiant
                .w-full.p-4.border-b(v-if="subclassSelected == 'Commander' && level >= 11")
                    p Martial Advantage
                .w-full.p-4.border-b(v-if="subclassSelected == 'Chieftain' && level >= 11")
                    p Wolfpack Tactics
                .w-full.p-4.border-b(v-if="subclassSelected == 'Packleader' && level >= 11")
                    p Fleeting Strikes
                .w-full.p-4.border-b(v-if="subclassSelected == 'Paragon' && level >= 11")
                    p Inspired Warrior
                .w-full.p-4.border-b(v-if="subclassSelected == 'Tactitian' && level >= 11")
                    p Tactical Strike
                .w-full.p-4.border-b(v-if="subclassSelected == 'Commander' && level >= 15")
                    p Bulwark
                .w-full.p-4.border-b(v-if="subclassSelected == 'Packleader' && level >= 15")
                    p Ghosts of the Wild
                .w-full.p-4.border-b(v-if="subclassSelected == 'Paragon' && level >= 15")
                    p Dauntless Resolve
            div(v-if="leadershipDice()")
                .w-full.p-4.border-t.mt-4.bg-gray-50.text-center
                    p Resource Tracker
                .w-full.p-4.border-b.bg-gray-50
                    p Leadership Dice: {{leadershipDice() + leadershipDieSize()}}
                .w-full.p-4.border-b
                    p #[template(v-for="n in leadershipDice()") #[input(type="checkbox")] #[span &nbsp;]]

            div(v-if="level >= 3 && (subclassSelected == 'Packleader' || subclassSelected == 'Noble')") 
                .w-full.p-4.border-t.bg-gray-50.mt-4.text-center
                    p Spellcasting
                .grid.grid-cols-2
                    .flex.flex-col.text-center
                        .p-4.bg-gray-50.border-b
                            p Spell Mod
                        .p-4(v-if="subclassSelected == 'Packleader'")
                            p #[span(v-if="hit(wis) >= 0") +]{{hit(wis)}}
                        .p-4(v-if="subclassSelected == 'Noble'")
                            p #[span(v-if="hit(cha) >= 0") +]{{hit(cha)}}
                    .flex.flex-col.text-center 
                        .p-4.bg-gray-50.border-b
                            p Spell DC
                        .p-4(v-if="subclassSelected == 'Packleader'")
                            p {{ DC(wis) }}
                        .p-4(v-if="subclassSelected == 'Noble'")
                            p {{ DC(cha) }}
                .w-full.p-4.border-y.bg-gray-50 
                    p Spells 
                .w-full.p-4.border-b(v-if="subclassSelected =='Noble' && level >= 3") 
                    p Command 
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 3") 
                    p Mold Earth
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 3 && packleaderCantrip") 
                    p {{packleaderCantrip}}
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 3") 
                    p Fog Cloud
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 3") 
                    p Snare
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 3 && packleaderSpell") 
                    p {{packleaderSpell}}
                .w-full.p-4.border-b(v-if="subclassSelected =='Noble' && level >= 7") 
                    p Bless
                .w-full.p-4.border-b(v-if="subclassSelected =='Noble' && level >= 7") 
                    p Heroism
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 7") 
                    p Pass Without Trace
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 18 && packleaderSpell2")
                    p {{packleaderSpell2}}
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 18 && packleaderSpell3")
                    p {{packleaderSpell3}}
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 18 && packleaderSpell4")
                    p {{packleaderSpell4}}
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 18 && packleaderSpell5")
                    p {{packleaderSpell5}}
                .w-full.p-4.border-b(v-if="subclassSelected =='Packleader' && level >= 18 && packleaderSpell6")
                    p {{packleaderSpell6}}

            .w-full.p-4.bg-gray-50.border-y.mt-4.text-center
                p Class Features
            template(v-for='feature in features')
                .w-full.p-4(v-if='feature.level <= level') 
                    p {{ feature.name }}
                .w-full.p-4.border-b(v-if='feature.level <= level')
                    p {{ feature.description }}
            
            div(v-if='subclassSelected')
                .w-full.p-4.bg-gray-50.border-y.mt-4.text-center
                    p Subclass Features
                template(v-for='feature in subclassFeatures()')
                    .w-full.p-4(v-if='feature.level <= level') 
                        p {{feature.name}}
                    .w-full.p-4.border-b(v-if='feature.level <= level')
                        p {{ feature.description }}
            
            div(v-if="raceSelected") 
                .w-full.p-4.text-center.bg-gray-50.border-t.mt-4
                    p Race Features
                template(v-for='feature in getRaceFeatures()')
                    .w-full.p-4.border-t(v-if='feature.level <= level') 
                        p {{feature.name}}
                    .w-full.p-4(v-if='feature.level <= level')
                        p {{ feature.description }} 
                template(v-for='feature in getSubraceFeatures()' v-if="subraceSelected")
                    .w-full.p-4.border-t(v-if='feature.level <= level')
                        p {{feature.name}}
                    .w-full.p-4(v-if='feature.level <= level')
                        p {{feature.description}}





</template>

<script>
export default {
    data() {
		return {
			name: "Jane Doe",
			level: 1,
			className: "Warlord",
			str: 10,
			dex: 10,
			con: 10,
			int: 10,
			wis: 10,
			cha: 10,
			tempHP: 0,
			saves: {},
			proficiencies: [],
			armorSelected: false,
			shieldSelected: false,
			weaponName: "",
			weaponDie: "",
			weaponIsRanged: false,
			weaponIsFinesse: false,
			offhandWeapon: false,
			offhandWeaponName: "",
			offhandWeaponDie: "",
			offhandWeaponIsRanged: false,
			offhandWeaponIsFinesse: false,
			packleaderCantrip: null,
			packleaderSpell: null,
			packleaderSpell2: null,
			packleaderSpell3: null,
			packleaderSpell4: null,
			packleaderSpell5: null,
			packleaderSpell6: null,
			features: [],
			subclassSelected: null,
			subclasses: {},
			armorTypes: [],
            showSheet: false,
            showEditor: true,
            races: {},
            raceSelected: false,
            subraceSelected: false,
            backgrounds: [
                'Acolyte'
            ],
            backgroundSelected: ''
		};
	},
	methods: {
		proficiency() {
			if (this.level >= 17) {
				return 6;
			} else if (this.level >= 13) {
				return 5;
			} else if (this.level >= 9) {
				return 4;
			} else if (this.level >= 5) {
				return 3;
			} else {
				return 2;
			}
		},
        mod(atr) {
            return Math.floor((atr - 10) / 2);
        },
		hp() {
            if (this.subraceSelected != 'Hill') {
			    return ((this.mod(this.con) + 8) + ((this.level - 1) * 5) + ((this.level - 1) * this.mod(this.con)));
            } else {
                return ((this.mod(this.con) + 9) + ((this.level - 1) * 6) + ((this.level - 1) * this.mod(this.con))); 
            }
		},
		currentHp() {
			return (
				this.mod(this.con) +
				8 +
				(this.level - 1) * 5 +
				(this.level - 1) * this.mod(this.con)
			);
		},
        hit(modifier) {
            return this.mod(modifier) + this.proficiency();
        },
		DC(modifier) {
			return this.mod(modifier) + this.proficiency() + 8;
		},
		leadershipDice() {
			if (this.subclassSelected == "Noble") {
				if (this.level >= 15) {
					return this.level + 3;
				} else if (this.level >= 9) {
					return this.level + 2;
				} else if (this.level >= 3) {
					return this.level + 1;
				} else if (this.level > 1) {
					return this.level;
				} else {
					return false;
				}
			} else {
				if (this.level > 1) {
					return this.level;
				} else {
					return false;
				}
			}
		},
		currentLeadershipDice() {
			if (this.subclassSelected == "Noble") {
				if (this.level >= 15) {
					return this.level + 3;
				} else if (this.level >= 9) {
					return this.level + 2;
				} else if (this.level >= 3) {
					return this.level + 1;
				} else if (this.level > 1) {
					return this.level;
				} else {
					return false;
				}
			} else {
				if (this.level > 1) {
					return this.level;
				} else {
					return false;
				}
			}
		},
		leadershipDieSize() {
			if (this.level >= 17) {
				return "d12";
			} else if (this.level >= 13) {
				return "d10";
			} else if (this.level >= 9) {
				return "d8";
			} else {
				return "d6";
			}
		},
		basicAC() {
			if (this.armorSelected) {
				if (
					this.armorSelected.dexCap &&
					this.armorSelected.dexCap < this.mod(this.dex)
				) {
					return this.armorSelected.ac + this.armorSelected.dexCap;
				} else {
					return this.armorSelected.ac + this.mod(this.dex);
				}
			} else {
				return 10 + this.mod(this.dex);
			}
		},
		AC() {
			if (this.subclassSelected == "Noble" && this.level >= 3) {
				if (this.mod(this.cha) > 2) {
					return this.basicAC() + 2;
				} else if (this.mod(this.cha) > 0) {
					return this.basicAC() + this.mod(this.cha);
				} else {
					return this.basicAC();
				}
			} else if (this.shieldSelected == true) {
				return this.basicAC() + 2;
			} else {
				return this.basicAC();
			}
		},
        save(modifier, save) {
            if (save == true) {
                return this.mod(modifier) + this.proficiency();
            } else {
                return this.mod(modifier);
            }
        },
		weaponAttackMod() {
			if (this.weaponIsFinesse || this.weaponIsRanged) {
				return this.proficiency() + this.mod(this.dex);
			} else {
				return this.proficiency() + this.mod(this.str);
			}
		},
		weaponDamageMod() {
			if (this.weaponIsFinesse || this.weaponIsRanged) {
				return this.mod(this.dex);
			} else {
				return this.mod(this.str);
			}
		},
		offhandWeaponAttackMod() {
			if (this.offhandWeaponIsFinesse || this.offhandWeaponIsRanged) {
				return this.proficiency() + this.mod(this.dex);
			} else {
				return this.proficiency() + this.mod(this.str);
			}
		},
		initiative() {
			if (this.subclassSelected == "Paragon") {
				return this.mod(this.dex) + this.mod(this.cha);
			} else if (this.subclassSelected == "Tactitian") {
				return this.mod(this.dex) + this.mod(this.int);
			} else {
				return this.mod(this.dex);
			}
		},
        stringToMod(string) {
            switch(string) {
                case "str":
                    return this.str;
                case "dex":
                    return this.dex;
                case "con":
                    return this.con;
                case "int":
                    return this.int;
                case "wis":
                    return this.wis;
                case "cha":
                    return this.cha;
            }
        },
        skillMod(stat, proficient, expert) {
            if (proficient) {
                if (expert) {
                    return this.mod(this.stringToMod(stat)) + this.proficiency() * 2;
                } else {
                    return this.mod(this.stringToMod(stat)) + this.proficiency();
                }
            } else {
                return this.mod(this.stringToMod(stat));
            }
        },
        toggleSheet() {
            if (this.showSheet) {
                this.showSheet = false;
                this.showEditor = true;
            } else {
                this.showSheet = true;
                this.showEditor = false;
            }
        },
        subclassFeatures() {
            if (this.subclassSelected != null) {
                return this.subclasses[this.subclassSelected.toLowerCase()].features;
            }
        },
        getSubraces() {
                return this.races[this.raceSelected.toLowerCase()].subraces;
        },
        getRaceFeatures() {
                return this.races[this.raceSelected.toLowerCase()].features;
        },
        getSubraceFeatures() {
            if (this.raceSelected && this.subraceSelected && this.subraceSelected.toLowerCase() in this.races[this.raceSelected.toLowerCase()].subraces) {
                return this.races[this.raceSelected.toLowerCase()].subraces[this.subraceSelected.toLowerCase()].features;
            }
        }
	},
	mounted() {
		fetch("./data/armor-min.json")
			.then((response) => response.json())
			.then((data) => (this.armorTypes = data));
		fetch("./data/warlordFeatures-min.json")
			.then((response) => response.json())
			.then((data) => (this.features = data));
		fetch("./data/warlordSubclasses-min.json")
			.then((response) => response.json())
			.then((data) => (this.subclasses = data));
		fetch("./data/proficiencies-min.json")
			.then((response) => response.json())
			.then((data) => (this.proficiencies = data));
		fetch("./data/warlordSaves-min.json")
			.then((response) => response.json())
			.then((data) => (this.saves = data));
        fetch("./data/races-min.json")
			.then((response) => response.json())
			.then((data) => (this.races = data));
	},
}
</script>

<style scoped>
@media print {
    body * {
        visibility: hidden;
    }
    #sheet * {
        visibility: visible;
    }
    #sheet {
        position: absolute;
        top: 2em;
        left: auto;
        right: auto;
    }
}

</style>

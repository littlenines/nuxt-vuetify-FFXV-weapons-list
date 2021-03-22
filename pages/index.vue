<template>
  <v-app>

    <v-data-table
      :headers = 'headers'
      :items = 'weapons'
      item-key = 'name'
      sort-by = 'calories'
      class = 'elevation-1'
      group-by = 'category'
      :search = 'search'
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Final Fantasy XV Weapons</v-toolbar-title>
          <v-divider class = 'mx-4' inset vertical></v-divider>
          <!-- search -->
          <v-text-field
            class = 'mr-5'
            v-model='search'
            append-icon = 'mdi-magnify'
            label = 'Search'
            single-line
            hide-details
          ></v-text-field>

          <v-dialog v-model='dialog' max-width = '500px'>
            <!-- Add button -->
            <template v-slot:activator='{ on, attrs }'>
              <v-btn
                color = 'primary'
                dark
                class = 'mb-2' 
                v-bind='attrs' 
                v-on='on'
              >
                Add Weapon
              </v-btn>
            </template>
            <!-- Edit/add card and cancel save components-->
            <v-card>
              <v-card-title>
                <span class ='headline'>{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>

                    <v-col cols = '12' sm = '6' md = '4'>
                      <v-text-field
                        v-model='editedItem.name'
                        label = 'Weapon name'
                      ></v-text-field>
                    </v-col>

                    <v-col cols='12' sm='6' md='4'>
                      <v-text-field
                        v-model='editedItem.attack'
                        label = 'Attack'
                      ></v-text-field>
                    </v-col>

                    <v-col cols = '12' sm = '6' md = '4'>
                      <v-text-field
                        v-model='editedItem.crit'
                        label = 'Critical'
                      ></v-text-field>
                    </v-col>

                    <v-col cols = '12' sm = '6' md = '4'>
                      <v-text-field
                        v-model='editedItem.mod'
                        label = 'Modifiers/Notes'
                      ></v-text-field>
                    </v-col>

                    <v-col cols = '12' sm = '6' md = '4'>
                      <v-text-field
                        v-model='editedItem.val'
                        label = 'Value'
                      ></v-text-field>
                    </v-col>

                    <v-col cols = '12' sm = '6' md = '4'>
                      <v-text-field
                        v-model='editedItem.obtain'
                        label = 'Obtain'
                      ></v-text-field>
                    </v-col>

                    <v-col cols = '12' sm = '6' md = '4'>
                      <v-text-field
                        v-model='editedItem.category'
                        label = 'Category'
                      ></v-text-field>
                    </v-col>

                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color = 'blue darken-1' text @click='close'>Cancel</v-btn>
                <v-btn color = 'blue darken-1' text @click='save'>Save</v-btn>
              </v-card-actions>

            </v-card>
          </v-dialog>
          <!-- delete action -->
          <v-dialog v-model='dialogDelete' max-width = '500px'>
            <v-card>
              <v-card-title>
                Are you sure you want to delete this item?
              </v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn color = 'blue darken-1' text @click='closeDelete'>Cancel</v-btn>
                <v-btn color = 'blue darken-1' text @click='deleteItemConfirm'>OK</v-btn>

              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <!-- edit and delete buttons -->
      <template v-slot:[`item.actions`]='{ item }'>
        <v-icon small class = 'mr-2' @click='editItem(item)'> mdi-pencil </v-icon>
        <v-icon small @click='deleteItem(item)'> mdi-delete </v-icon>
      </template>
      <!-- custom group_by -->
      <template v-slot:[`group.header`]='{ items, isOpen, toggle }'>
        <th colspan = '7'>
          <v-icon @click='toggle'>
            {{ isOpen ? 'mdi-minus' : 'mdi-plus' }}
          </v-icon>
          {{ items[0].category }}
        </th>
      </template>

    </v-data-table>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    search: '',
    headers: [
      {
        text: 'Name',
        align: 'start',
        sortable: false,
        value: 'name',
      },
      { text: 'Attack', value: 'attack' },
      { text: 'Critical', value: 'crit' },
      { text: 'Modifiers/Notes', value: 'mod', sortable: false },
      { text: 'Value', value: 'val', sortable: false },
      { text: 'Obtain', value: 'obtain', sortable: false },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
    weapons: [
      {
        id: '1',
        name: 'Engine Blade',
        attack: 28,
        crit: 3,
        mod: 'Max MP: +5 , Magic: +5 , Absorbs elemental energy when dealing the finishing blow to an enemy.',
        val: 'Buy: No Sell: 1',
        obtain: 'Default: Noctis at start of game.',
        category: 'Swords',
      },
      {
        id: '2',
        name: 'Broadsword',
        attack: 42,
        crit: 3,
        mod: 'Max MP: +6 , +2% Critical Rate per combo hit',
        val: 'Buy: 150  Sell: 75',
        obtain: 'Shop: Ch.1–8: Hammerhead, Galdin Quay, Prairie Outpost',
        category: 'Swords',
      },
      {
        id: '3',
        name: 'Engine Blade II',
        attack: 42,
        crit: 3,
        mod: 'Max MP: +15 , Magic: +12 , Absorbs elemental energy when dealing the finishing blow to an enemy',
        val: 'Buy: No  Sell: 1',
        obtain: 'Upgrade: Give Engine Blade to Cid along with a Rusted Bit',
        category: 'Swords',
      },
      {
        id: '4',
        name: 'Airstep Sword',
        attack: 83,
        crit: 3,
        mod: 'Max MP: +7 , Halves MP consumption in mid-air',
        val: 'Buy: 300 Sell: 150',
        obtain: 'Shop: Wiz Chocobo Post, Coernix Station - Cauthess',
        category: 'Swords',
      },
      {
        id: '5',
        name: 'Rune Saber',
        attack: 103,
        crit: 3,
        mod: 'Max HP: +48 , Max MP: +8 , Vitality: +8 , Magic: +12 , Spirit: +9',
        val: 'Buy: 600 Sell: 300',
        obtain: 'Default: Noctis in Close Encounter of the Terra Kind Shop: Taelpar Rest Area, Lestallum',
        category: 'Swords',
      },
      {
        id: '6',
        name: 'Flame Tongue',
        attack: 117,
        crit: 3,
        mod: 'Max MP: +7 , Fire Resistance: 28% , Inflicts Fire-based damage',
        val: 'Buy: 800 Sell: 400',
        obtain: 'Shop: Taelpar Rest Area, Lestallum',
        category: 'Swords',
      },
      {
        id: '7',
        name: 'Engine Blade III',
        attack: 207,
        crit: 3,
        mod: 'Max MP: +25 , Magic: +18 , Absorbs elemental energy when dealing the finishing blow to an enemy',
        val: 'Buy: No Sell: 1',
        obtain: 'Upgrade: Give Engine Blade II to Cid along with a Glass Gemstone',
        category: 'Swords',
      },
      {
        id: '8',
        name: 'Durandal',
        attack: 232,
        crit: 3,
        mod: 'Max MP: +11 , Dark Resistance: 33% , Light-elemental',
        val: 'Buy: 10000 Sell: 5000',
        obtain: 'Find: Costlemark Tower (Map) Shop: Meldacio Hunter HQ, Ch.14: Hammerhead, Kingsglaive Base Camp (Windows/Royal Edition)',
        category: 'Swords',
      },
      {
        id: '9',
        name: 'Ultima Blade',
        attack: 364,
        crit: 3,
        mod: 'Max MP: +40 , Magic: +30 , Absorbs elemental energy when dealing the finishing blow to an enemy',
        val: 'Buy: No Sell: 1',
        obtain: 'Upgrade: Give Engine Blade III to Cid along with a Sturdy Helixhorn',
        category: 'Swords',
      },
      {
        id: '10',
        name: 'Balmung',
        attack: 446,
        crit: 3,
        mod: 'Max MP: +11 , Damage reduction with lower MP: -4% damage for every percentage point reduction',
        val: 'Buy: No Sell: 10000',
        obtain: 'Find: Ch.15: Steyliff Grove maze (Level 29)',
        category: 'Swords',
      },
      {
        id: '11',
        name: 'Two-handed Sword',
        attack: 48,
        crit: 2,
        mod: 'Max HP: +53 , +15% damage per additional enemy within 65 feet radius (max + 100%)',
        val: 'Buy: 50 Sell: 25',
        obtain: 'Default: Gladiolus, Noctis at start of game Shop: Ch.1–8: Hammerhead',
        category: 'Broadswords',
      },
      {
        id: '12',
        name: 'War Sword',
        attack: 78,
        crit: 2,
        mod: 'Max HP: +65 ,10% chance to inflict Compromised',
        val: 'Buy: 150 Sell: 75',
        obtain: 'Shop: Ch.1–8: Hammerhead, Galdin Quay, Prairie Outpost',
        category: 'Broadswords',
      },
      {
        id: '13',
        name: 'Claymore',
        attack: 156,
        crit: 2,
        mod: 'Max HP: +98 , Vitality: +32',
        val: 'Buy: 600 Sell: 300',
        obtain: 'Shop: Taelpar Rest Area, Lestallum',
        category: 'Broadswords',
      },
      {
        id: '14',
        name: 'Thunderbolt',
        attack: 345,
        crit: 2,
        mod: 'Max HP: +246 , Lightning Resistance: 29%, Inflicts Lightning-based damage',
        val: 'Buy: 5000 Sell: 2500',
        obtain: 'Find: Malmalam Thicket (before the first clearing; Map), Pitioss Ruins , Shop: Altissia, Cartanica, Tenebrae, Zegnautus Keep',
        category: 'Broadswords',
      },
      {
        id: '15',
        name: 'Afrosword',
        attack: 503,
        crit: 2,
        mod: 'Max HP: +287 , Spirit: +54 , Changes battle music to Timed Quest music when equipped to Noctis or Gladiolus and controlling them',
        val: 'Buy: No Sell: No',
        obtain: 'Trade: 1000 QP at Prize Exchange at Timed Quests (only one)',
        category: 'Broadswords',
      },
      {
        id: '16',
        name: 'Avengers',
        attack: 20,
        crit: 8,
        mod: 'Max MP: +4 , Magic: +5 , Critical bonus when the wielder has low HP: Critical + (5 + 1.5n)% where n is every percentage point reduction under 30% HP',
        val: 'Buy: 150 Sell: 75',
        obtain: 'Find: North of Three Valleys (Leide; Map) , Shop: Ch.1–8: Hammerhead, Galdin Quay, Prairie Outpost',
        category: 'Daggers',
      },
      {
        id: '17',
        name: 'Cutlasses',
        attack: 58,
        crit: 8,
        mod: 'Max MP: +6 , Magic: +10 , 10% chance to inflict Enfeebled',
        val: 'Buy: 300 Sell: 150',
        obtain: 'Shop: Wiz Chocobo Post, Coernix Station - Cauthess',
        category: 'Daggers',
      },
      {
        id: '18',
        name: 'Mythril Knives',
        attack: 62,
        crit: 8,
        mod: 'Max MP: +6 , Magic: +50',
        val: 'Buy: 600 Sell: 300',
        obtain: 'Default: Noctis in Close Encounter of the Terra Kind , Shop: Taelpar Rest Area, Lestallum',
        category: 'Daggers',
      },
      {
        id: '19',
        name: 'Orichalcum',
        attack: 223,
        crit: 8,
        mod: 'Max MP: +9 , Magic: +20 , Light-elemental',
        val: 'Buy: 10000 Sell: 5000',
        obtain: 'Find: Costlemark Tower (across the beam near the start, top of the spiral; Map) , Shop: Meldacio Hunter HQ',
        category: 'Daggers',
      },
    ],
    editedIndex: -1,
    editedItem: {
      name: '',
      attack: 0,
      crit: 0,
      mod: '',
      val: '',
      obtain: '',
    },
    defaultItem: {
      name: '',
      attack: 0,
      crit: 0,
      mod: '',
      val: '',
      obtain: '',
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'New Weapon' : 'Edit Weapon';
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  methods: {

    editItem(item) {
      this.editedIndex = this.weapons.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.weapons.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.weapons.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.weapons[this.editedIndex], this.editedItem);
      } else {
        this.weapons.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>


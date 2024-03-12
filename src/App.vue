<script lang="ts">
  const stringCollator = new Intl.Collator("en", { sensitivity: 'accent', usage: 'sort' });
  const isEmpty = (val: any) => {
    return val === null || val === undefined || (typeof val === 'string' && val.trim() === '')
  };
  const sortNumbers = (sortA: any, sortB: any) => {
    [sortA, sortB] = [sortA, sortB].map(s => s != null ? s.toString().toLocaleLowerCase() : s);
    if (sortA === sortB) return 0;
    if (isEmpty(sortA) && isEmpty(sortB)) return 0;
    if (isEmpty(sortA)) return -1;
    if (isEmpty(sortB)) return 1;

    sortA = sortA.replace(/[^0-9]/g, '');
    sortB = sortB.replace(/[^0-9]/g, '');

    if (!isNaN(sortA) && !isNaN(sortB)) return Number(sortA) - Number(sortB);
    return stringCollator.compare(sortA, sortB);
  };

  type SortItem = {
    key: string;
    order?: 'asc' | 'desc';
  };
  type Header = {
    readonly title?: string;
    readonly align?: "center" | "end" | "start";
    readonly sortable: boolean;
    readonly key?: string;
    readonly sort?: (a: any, b: any) => number;
  };
  export default {
    data () {
      const sortBy: readonly SortItem[] = [{ key: 'calories', order: 'asc' }];
      const headers: readonly Header[] = [
          {
            title: 'Dessert (100g serving)',
            align: 'start',
            sortable: false,
            key: 'name',
          },
          {
            title: 'Calories',
            sortable: true,
            key: 'calories'
          },
          {
            title: 'Fat (g)',
            sortable: true,
            key: 'fat'
          },
          {
            title: 'Carbs (g)',
            sortable: true,
            key: 'carbs'
          },
          {
            title: 'Protein (g)',
            sortable: true,
            key: 'protein'
          },
          {
            title: 'Iron (%)',
            sortable: true,
            key: 'iron',
            sort: sortNumbers
          },
        ];
      const desserts = [
          {
            name: 'Frozen Yogurt',
            calories: 200,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%',
          },
          {
            name: 'Ice cream sandwich',
            calories: 200,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%',
          },
          {
            name: 'Eclair',
            calories: 300,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%',
          },
          {
            name: 'Cupcake',
            calories: 300,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%',
          },
          {
            name: 'Gingerbread',
            calories: 400,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%',
          },
          {
            name: 'Jelly bean',
            calories: 400,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%',
          },
          {
            name: 'Lollipop',
            calories: 400,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%',
          },
          {
            name: 'Honeycomb',
            calories: 400,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%',
          },
          {
            name: 'Donut',
            calories: 500,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%',
          },
          {
            name: 'KitKat',
            calories: 500,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '',
          },
        ];
      return {
        sortBy: sortBy,
        headers: headers,
        desserts: desserts,
      };
    },
  }
</script>

<template>
  <v-app>
    <v-sheet
      class="pa-2"
      width="894"
    >
      <v-data-table
        v-model:sort-by="sortBy"
        :headers="headers"
        :items="desserts"
      ></v-data-table>
    </v-sheet>
  </v-app>
</template>

<style scoped>
</style>

<template>
	<v-card class="pb-2">
		<v-card-title class="pb-0">
			{{ $t('panel.settingsEndstops.caption') }}
		</v-card-title>

		<v-simple-table v-show="endstops.length" dense>
			<template v-slot:default>
				<thead>
					<tr>
						<th class="text-center">{{ $t('panel.settingsEndstops.index') }}</th>
						<th class="text-center">{{ $t('panel.settingsEndstops.triggered') }}</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="endstop in endstops" :key="endstop.index">
						<td class="text-center">{{ endstop.index }}</td>
						<td class="text-center">{{ endstop.triggered ? $t('generic.yes') : $t('generic.no') }}</td>
					</tr>
				</tbody>
			</template>
		</v-simple-table>

		<div v-show="!endstops.length" class="mt-1 text-center">
			{{ $t('$vuetify.noDataText') }}
		</div>
	</v-card>
</template>

<script>
'use strict'

import { mapState, mapMutations } from 'vuex'

export default {
	computed: {
		...mapState(['selectedMachine']),
		...mapState('machine/model', ['sensors']),
		endstops() {
			return this.sensors.endstops.map((endstop, index) => ({
				index,
				triggered: endstop.triggered
			}));
		}
	},
	data() {
		return {
			active: true
		}
	},
	activated() {
		this.active = true;
		this.setHighVerbosity();
	},
	deactivated() {
		this.active = false;
		this.setNormalVerbosity();
	},
	methods: mapMutations('machine', ['setHighVerbosity', 'setNormalVerbosity']),
	watchers: {
		selectedMachine() {
			if (this.active) {
				this.setHighVerbosity();
			}
		}
	}
}
</script>

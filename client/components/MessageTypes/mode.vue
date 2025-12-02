<template>
	<span class="content">
		<Username :user="message.from" />
		sets mode
		<span>{{ formattedMode }}</span>
	</span>
</template>

<script lang="ts">
import {defineComponent, PropType, computed} from "vue";
import {ClientNetwork, ClientMessage} from "../../js/types";
import ParsedMessage from "../ParsedMessage.vue";
import Username from "../Username.vue";

export default defineComponent({
	name: "MessageTypeMode",
	components: {
		ParsedMessage,
		Username,
	},
	props: {
		network: {
			type: Object as PropType<ClientNetwork>,
			required: true,
		},
		message: {
			type: Object as PropType<ClientMessage>,
			required: true,
		},
	},
	setup(props) {
		const formattedMode = computed(() => {
			const modeMap: Record<string, string> = {
				'+v': 'VOICE on',
				'-v': 'removes VOICE from',
				'+o': 'OP on',
				'-o': 'removes OP from',
				'+h': 'HALF-OP on',
				'-h': 'removes HALF-OP from',
				'+a': 'ADMIN on',
				'-a': 'removes ADMIN from',
				'+q': 'OWNER on',
				'-q': 'removes OWNER from',
			};
			
			let text = (props.message as any).text || '';
			
			// Replace mode symbols with readable names
			Object.keys(modeMap).forEach((mode) => {
				text = text.replace(mode, modeMap[mode]);
			});
			
			return text;
		});
		
		return {
			formattedMode
		};
	}
});
</script>

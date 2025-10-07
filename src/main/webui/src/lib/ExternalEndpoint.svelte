<script>
import {faArrowUpRightFromSquare} from '@fortawesome/free-solid-svg-icons';
import Fa from 'svelte-fa';

import { appConfig } from './stores/appConfig.js';

export let jobName;
export let type;
export let title;
export let ingressUrl = null;

$: endpointPathPattern = $appConfig?.endpointPathPatterns?.[type];

function processEndpointPathPattern(pattern, jobName) {
    return pattern.replace('$jobName', jobName);
}

function getEndpointUrl() {
    // For flink-ui type, use ingressUrl if available
    if (type === 'flink-ui' && ingressUrl) {
        // Add https:// protocol if not present
        return ingressUrl.startsWith('http') ? ingressUrl : `https://${ingressUrl}`;
    }
    // Fall back to pattern-based approach
    if (endpointPathPattern) {
        return processEndpointPathPattern(endpointPathPattern, jobName);
    }
    return null;
}

$: endpointUrl = getEndpointUrl();
</script>

{#if endpointUrl}
    <a href="{endpointUrl}" target="_blank" class="text-blue-600 mr-1">
        {title} <Fa fw icon={faArrowUpRightFromSquare} />
    </a>
{/if}

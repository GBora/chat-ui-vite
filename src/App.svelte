<script lang="ts">
    import { TabContent, TabPane } from 'sveltestrap';
    import QuestionPane from './components/QuestionPane.svelte';
    import SourcesPane from './components/SourcesPane.svelte';
</script>
    
  <div class="container mx-auto w-50 mt-5">

    <TabContent>
        <TabPane tabId="bravo" tab="Questions" active>
            <QuestionPane></QuestionPane>
        </TabPane>
        <TabPane tabId="charlie" tab="Sources">
            <SourcesPane></SourcesPane>
        </TabPane>
    </TabContent>
  </div>
  
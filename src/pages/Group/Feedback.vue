<script>
import { connect } from 'vuex-connect'
import PickupFeedback from '@/components/Statistics/PickupFeedback'

export default connect({
  gettersToProps: {
    pickups: 'pickups/feedbackPossible',
    existingFeedback: 'feedback/all',
    status: 'feedback/saveStatus',
  },
  actionsToEvents: {
    save: 'feedback/save',
  },
})('PickupFeedback', PickupFeedback)
</script>

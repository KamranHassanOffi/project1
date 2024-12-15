# project1

import java.time.LocalTime;
import java.util.LinkedList;
import java.util.List;

class Event {
    String title;
    LocalTime startTime;
    LocalTime endTime;
    int priority;

    public Event(String title, LocalTime startTime, LocalTime endTime, int priority) {
        this.title = title;
        this.startTime = startTime;
        this.endTime = endTime;
        this.priority = priority;
    }

    @Override
    public String toString() {
        return "Event{" +
               "title='" + title + '\'' +
               ", startTime=" + startTime +
               ", endTime=" + endTime +
               ", priority=" + priority +
               '}';
    }
}

public class EventScheduler {
    private LinkedList<Event> events;

    public EventScheduler() {
        events = new LinkedList<>();
    }

    public void addEvent(Event event) {
        events.add(event);
        events.sort((e1, e2) -> e1.startTime.compareTo(e2.startTime));
    }

    public void removeEvent(String title) {
        events.removeIf(event -> event.title.equalsIgnoreCase(title));
    }

    public Event searchEvent(String title) {
        for (Event event : events) {
            if (event.title.equalsIgnoreCase(title)) {
                return event;
            }
        }
        return null;
    }

    public List<Event> getOverlappingEvents() {
        List<Event> overlapping = new LinkedList<>();
        for (int i = 0; i < events.size(); i++) {
            for (int j = i + 1; j < events.size(); j++) {
                if (events.get(i).endTime.isAfter(events.get(j).startTime) &&
                    events.get(i).startTime.isBefore(events.get(j).endTime)) {
                    if (!overlapping.contains(events.get(i))) overlapping.add(events.get(i));
                    if (!overlapping.contains(events.get(j))) overlapping.add(events.get(j));
                }
            }
        }
        return overlapping;
    }

    public void displayEvents() {
        events.forEach(System.out::println);
    }

    public static void main(String[] args) {
        EventScheduler scheduler = new EventScheduler();

        scheduler.addEvent(new Event("Meeting", LocalTime.of(9, 0), LocalTime.of(10, 0), 1));
        scheduler.addEvent(new Event("Lunch", LocalTime.of(12, 0), LocalTime.of(13, 0), 3));
        scheduler.addEvent(new Event("Call", LocalTime.of(10, 30), LocalTime.of(11, 30), 2));

        System.out.println("All Events:");
        scheduler.displayEvents();

        System.out.println("\nOverlapping Events:");
        scheduler.getOverlappingEvents().forEach(System.out::println);

        System.out.println("\nSearch Event (Call):");
        System.out.println(scheduler.searchEvent("Call"));

        System.out.println("\nRemove Event (Lunch):");
        scheduler.removeEvent("Lunch");
        scheduler.displayEvents();
    }
}

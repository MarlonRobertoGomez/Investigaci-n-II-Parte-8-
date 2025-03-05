Observer
// Interfaz de observador
public interface IObserver {
    void Actualizar(string mensaje);
}

// Sujeto
public class Sujeto {
    private List<IObserver> observadores = new List<IObserver>();

    public void AgregarObservador(IObserver observador) {
        observadores.Add(observador);
    }

    public void Notificar(string mensaje) {
        foreach (var obs in observadores)
            obs.Actualizar(mensaje);
    }
}

// Observador concreto
public class Cliente : IObserver {
    private string nombre;

    public Cliente(string nombre) {
        this.nombre = nombre;
    }

    public void Actualizar(string mensaje) {
        Console.WriteLine($"{nombre} recibió el mensaje: {mensaje}");
    }
}

// Uso
class Program {
    static void Main() {
        Sujeto tienda = new Sujeto();
        Cliente cliente1 = new Cliente("Juan");
        Cliente cliente2 = new Cliente("María");

        tienda.AgregarObservador(cliente1);
        tienda.AgregarObservador(cliente2);

        tienda.Notificar("¡Nuevo producto disponible!");
    }
}

Chain of Responsabilty
public abstract class Approver
{
    protected Approver? NextApprover;

    public void SetNext(Approver nextApprover)
    {
        NextApprover = nextApprover;
    }

    public abstract void ProcessRequest(decimal amount);
}
public class Manager : Approver
{
    public override void ProcessRequest(decimal amount)
    {
        if (amount <= 1000)
        {
            Console.WriteLine($"🟢 Manager aprobó el gasto de ${amount}");
        }
        else if (NextApprover != null)
        {
            NextApprover.ProcessRequest(amount);
        }
        else
        {
            Console.WriteLine($"❌ Solicitud de ${amount} no aprobada.");
        }
    }
}

public class Director : Approver
{
    public override void ProcessRequest(decimal amount)
    {
        if (amount <= 5000)
        {
            Console.WriteLine($"🟢 Director aprobó el gasto de ${amount}");
        }
        else if (NextApprover != null)
        {
            NextApprover.ProcessRequest(amount);
        }
        else
        {
            Console.WriteLine($"❌ Solicitud de ${amount} no aprobada.");
        }
    }
}

public class CEO : Approver
{
    public override void ProcessRequest(decimal amount)
    {
        if (amount <= 20000)
        {
            Console.WriteLine($"🟢 CEO aprobó el gasto de ${amount}");
        }
        else
        {
            Console.WriteLine($"❌ Solicitud de ${amount} rechazada por ser demasiado alta.");
        }
    }
}
class Program
{
    static void Main()
    {
        Approver manager = new Manager();
        Approver director = new Director();
        Approver ceo = new CEO();

        manager.SetNext(director);
        director.SetNext(ceo);

        // Pruebas con diferentes montos
        manager.ProcessRequest(500);   // Manager puede aprobar
        manager.ProcessRequest(3000);  // Director puede aprobar
        manager.ProcessRequest(10000); // CEO puede aprobar
        manager.ProcessRequest(50000); // Nadie puede aprobar
    }
}
🟢 Manager aprobó el gasto de $500
🟢 Director aprobó el gasto de $3000
🟢 CEO aprobó el gasto de $10000
❌ Solicitud de $50000 rechazada por ser demasiado alta.
